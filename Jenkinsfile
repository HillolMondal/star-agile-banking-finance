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
                sh 'docker build -t hillol111/projectfinance:v2 .'
                sh 'docker images'
            }
        }
        }
        stage('Docker login') {
            steps {
                  withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                     sh "echo $PASS | docker login -u $USER --password-stdin"
                     sh 'docker push hillol111/projectfinance:v2'
                }
            }
            }
     stage('Deploy on Ansible') {
            steps {
                ansiblePlaybook become: true, credentialsId: 'Ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
            }
        }
    }
}
