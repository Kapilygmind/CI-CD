# vim /etc/sudoers
# jenkins ALL=(ALL) NOPASSWD: ALL

pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Kapilygmind/CI-CD.git']])
                sh 'mvn clean install'
            }
        }
    
    stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t kapilrjpt523/devops-integration .'
                }
            }
        }
    stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u kapilrjpt523 -p ${dockerhubpwd}'
                   sh 'docker push kapilrjpt523/devops-integration'
                }
            }
        }
    stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
   
