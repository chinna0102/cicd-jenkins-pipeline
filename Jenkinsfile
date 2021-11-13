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
