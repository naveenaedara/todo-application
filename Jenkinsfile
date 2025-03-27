pipeline{
    agent any
    tools{
        maven 'Maven 3.8.5'
    }
    environment{
        DOCKERHUB_CREDENTIALS=credentials('docker-hub-credentials')
        DOCKERHUB_USERNAME="naveenaedara1"
        IMAGE_NAME="todo-application-image"
        IMAGE_TAG="latest"  
    }
    stages{
        stage('checkout the code'){
            steps{
                git branch: 'main', url: 'https://github.com/naveenaedara/todo-application.git'
            }     

        }
        stage('build stage'){
            steps{
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('build the image'){
            steps{
                script{
                    sh "docker build -t $DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG ."
                }
            }
        }
        stage('push the image to dockerhub'){
          steps{
            script{
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                sh "docker push $DOCKERHUB_USERNAME/$IMAGE_NAME:$IMAGE_TAG"
            }
          }
        }
        stage('deploy app using docker compose'){
            steps{
                script{
                    sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
                    sh '''
                    docker compose version
                    docker compose up -d
                    docker compose ps
                    '''
                }
            }
        }
    }   
}    

