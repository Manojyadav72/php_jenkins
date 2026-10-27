pipeline {
    agent any

    environment {
        IMAGE_NAME = 'manoj9627/php-jenkins-demo'
        CONTAINER_NAME = 'php-jenkins-app'
        APP_PORT = '9000'
    }
    stages {
        stage('checkout') {
            steps{
                git branch: 'main', url: 'https://github.com/Manojyadav72/php_jenkins.git'
            }
        }
        stage('build docker image') {
           sh '''
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest .
           '''
        }
        stage('Test PHP Application') {
            steps {
                sh '''
                    docker run -d --rm --name php-test-${BUILD_NUMBER} -p 0.0.0.0:9000:80 ${IMAGE_NAME}:${BUILD_NUMBER}
                '''
            }
        }   
        stage('PUSH'){
            steps {
                withCredentials([
                    usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_USER')
                ])
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER_NAME} || true
                    docker run -d --name ${CONTAINER_NAME} -p ${APP_PORT}:80 ${IMAGE_NAME}:${BUILD_NUMBER}
                       
                   '''
                }
        }
    }
}