pipeline {
    agent {
        label 'agent01'
    }
    environment { 
           DOCKER_IMAGE_NAME = "srinivassayyapureddy/train-schedule"
           DOCKERHUB_CREDENTIALS = credentials('396dbd1d-1585-49f6-98a0-eb46dda83897')
    }  
    stages {
        stage('Clone MS-Repo') {
            steps {
                script 
                {
                    sh '''
                    echo "cloning the repo"
                    git clone https://github.com/srinivassayyapureddy/cicd-pipeline-train-schedule-autodeploy.git
                    '''
                 
                }
            }				
        }
        stage('Build') {
            steps { 
                script {
                sh '''
                echo 'Gradle Build Started'
                ./gradlew clean build
               
               '''
                }   
            }
        }
        stage('Containerize') {
            steps{
                
                sh '''
                  
                 echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                 docker build -t $DOCKER_IMAGE_NAME cicd-pipeline-train-schedule-autodeploy/Dockerfile .
                 docker push $DOCKER_IMAGE_NAME:latest
                 docker rmi $DOCKER_IMAGE_NAME:latest
                 
                 
                 '''
            }
        }
        stage ('K8S Deploy'){
            steps{
                sh ''' 
                  
                  kubectl get pods
                  kubectl apply -f .cicd-pipeline-train-schedule-autodeploy/train-schedule-kube.yml
                  '''

            } 
        }
    }
}

