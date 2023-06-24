pipeline{
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/rishi0803/Health-care.git']])
            }
        }
        stage('Build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('test'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Build image'){
            steps{
                script{
                    sh 'docker build -t rishi0803/healthcare-project .'
                }
            }
        }
        stage('push docker image'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u rishi0803 -p ${dockerhubpwd}'    
}
                    sh 'docker push rishi0803/healthcare-project'
                    
                }
            }
        }
        stage('ansible playbook'){
            steps{
                
               ansiblePlaybook credentialsId: 'jenkinsAnsible', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'hosts', playbook: 'ansible-playbook.yml'
            }
        }
    }
}
