pipeline{
    agent any

    environment {
        IMAGE_NAME='tanay25/php-jenkins-demo'
        CONTAINER_NAME='php-jenkins-app'
        APP_PORT='9000'
    }
    stages{
        stage('Checkout'){
            steps{
                git branch:'main',url:'https://github.com/tanay25/phpjenkins.git'
            }
        }
        stage('Build Docker Image'){
            steps{
                sh '''
                    docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest .
                '''
            }
        }
        
        stage('PUSH'){
            steps{
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKERHUB_PASSWORD'
                    )
                ])
                {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docekr push tanay25/${IMAGE_NAME}:latest
                    '''
                }
            }
        }
        stage('Deploy'){
            steps{
                sh '''
                    docker rm -f ${CONTAINER_NAME} || true
                    docekr run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:80 ${IMAGE_NAME}:${BUILD_NUMBER}

                '''
            }
        }
    }
}