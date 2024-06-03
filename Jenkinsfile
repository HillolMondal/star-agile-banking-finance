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
                    sh 'docker build -t hillol111/projectfinance .'
            }
        }
        stage('login docker hub'){
            steps{
                withCredentials([string(credentialsId: 'dockerhub-pass', variable: 'dockerhub-pass')])
                sh 'docker login -u hillol111 -p ${dockerhub-pass}'
                }
            }
 stage('docker tag and push'){
            steps{
                sh 'docker tag projectfinance hillol111/projectfinance:v1'
                sh 'docker push hillol111/projectfinance:v1'
                }
            }
     stage('Deploy on Ansible') {
            steps {
                ansiblePlaybook become: true, credentialsId: 'Ansible', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: '/home/ubuntu/star-agile-banking-finance/ansible-playbook.yml', vaultTmpPath: ''
            }
        }
    }
}
