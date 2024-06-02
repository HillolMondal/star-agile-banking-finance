pipeline {
    agent any
    stages{
        stage('build project'){
            steps{
                git url:'https://github.com/HillolMondal/star-agile-banking-finance.git/', branch: "master"
                sh 'mvn clean package'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t hillol111/projectfinance:v1 .'
                    sh 'docker images'
                }
            }
        }
        stage('login docker hub'){
            steps{
                withcrendentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')])
                sh "echo $PASS | docker login -u $USER --password-stdin"    
                sh 'docker push hillol111/projectfinance:v1'
                }
            }
        }
     stage('Deploy') {
            steps {
                sh 'sudo docker run -itd --name financeproject -p 8088:8081 hillol111/projectfinance:v1'
                }
            }
    }
}
