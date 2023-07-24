pipeline {
    agent any

    tools {
        maven 'M3'
    }
    environment {
        APP_NAME = 'registry-server' // Replace with your application name
        IMAGE_TAG = 'latest' // or you can use a dynamic tag, for example, using ${env.BUILD_ID}
        DOCKER_HUB_REPO = 'sk4k/registry-container' // Replace with your Docker Hub username and repository name
    }

    stages {
        stage('Checkout') {
                steps {
                    git credentialsId: 'github-credentials', url: 'https://github.com/shahmeerk/s-registryserver.git'
             }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_HUB_REPO}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'Lousy--97', usernameVariable: 'sk4k')]) {
                        sh '''
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        docker push ${DOCKER_HUB_REPO}:${IMAGE_TAG}
                        '''
                    }
                }
            }
        }
    }
}
