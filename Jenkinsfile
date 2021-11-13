pipeline{
    agent any
    environment {
        DOCKER_TAG = "getVersion()"
    }

    stages{
        stage('SCM'){
            steps{
                git 'https://github.com/chinna0102/cicd-jenkins-pipeline.git'
                
            }
        }
        
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
            }
        }
        
        stage('Docker Build'){
            steps{
                sh "docker build . -t chinna0102/hariapp "
            }
        }
        
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker-hub', variable: 'dockerHubPwd')]) {
                    sh "docker login -u chinna0102 -p ${dockerHubPwd}"
                }
                
                sh "docker push chinna0102/hariapp"
            }
        }
        
        
        stage('Docker Deploy'){
            steps{
           
               ansiblePlaybook credentialsId: 'cicd-pipeline', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker-ansible.yaml'
            }
                
        }
    }
    
}

def getVersion(){
    def commitHash = sh returnStdout: true, script: 'git rev-parse --short HEAD'
    return commitHash
}
