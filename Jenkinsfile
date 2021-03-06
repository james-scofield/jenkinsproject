pipeline{    
    agent { label "master" }    
    
    environment {
        ECR_REGISTRY = "468266085420.dkr.ecr.us-east-1.amazonaws.com"
        APP_REPO_NAME = "james/phonebook-app"
        AWS_REGION = "us-east-1"
        }   
        
        
    stages {        
             
             
        stage('Create ECR Repo') {
            steps {
                echo 'Creating ECR Repository' 
                sh """
                aws ecr create-repository \
                    --repository-name ${APP_REPO_NAME} \
                    --image-scanning-configuration scanOnPush=false \
                    --image-tag-mutability MUTABLE \
                    --region ${AWS_REGION}
                """
            }
        }        
        
        stage('Build App Docker Images'){
            steps {
                echo 'Building app Docker images'
                sh """docker build --force-rm -t "${ECR_REGISTRY}/${APP_REPO_NAME}:latest" ."""
                sh """docker image ls"""
            }
        }        
        
        stage('Push Docker Images to ECR Repo'){
            steps {
                echo 'Pushing images to ECR'
                sh "aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REGISTRY}"
                sh 'docker push "${ECR_REGISTRY}/${APP_REPO_NAME}:latest"'
            }
        }        
        
        stage('Create Infrastructure for the App'){
            steps {
                echo 'Creating Docker Swarm'
            }
        }        
        
        stage('Test the Infrastructure'){
            steps {
                echo 'Testing if Docker Swarm is ready or not'
            }
        }        
        
        stage('Deploy the App on Docker Swarm'){
            steps {
                echo 'Deploying the app'
            }
        }        
        
        stage('Test the application'){
            steps {
                echo 'Check if the application is ready or not'
            }
        }
    }    
    
    post {

        always {
            echo 'Deleting all local images'
            sh 'docker image prune -af'
        }        
        
        failure {
            echo 'Deleting the image repository on ECR due to failure'
            echo 'Deleting the cloudformation stack due to failure'
        }
    }
}