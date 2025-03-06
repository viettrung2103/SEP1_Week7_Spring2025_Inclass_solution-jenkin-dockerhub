pipeline {
    agent any
     environment {
            // Define Docker Hub credentials ID
            DOCKERHUB_CREDENTIALS_ID = 'Docker_Hub'
            // Define Docker Hub repository name
            DOCKERHUB_REPO = 'viettrung21/viettrung21/w7-inclass-jenkin-dockerhub-test'
            // Define Docker image tag
            DOCKER_IMAGE_TAG = 'latest_v1'
        }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/viettrung2103/SEP1_Week7_Spring2025_Inclass_solution-jenkin-dockerhub.git'
            }
        }
        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
        stage('Code Coverage') {
            steps {
                bat 'mvn jacoco:report'
            }
        }
        stage('Publish Test Results') {
            steps {
                junit '**/target/surefire-reports/*.xml'
            }
        }
        stage('Publish Coverage Report') {
            steps {
                jacoco()
            }
        }

         stage('Build Docker Image') {
                    steps {
                        // Build Docker image
                        script {
                             bat "docker build -t ${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG} ."
                        }
                    }
                }
                stage('Push Docker Image to Docker Hub') {
                    steps {
                        // Push Docker image to Docker Hub
                        script {
                            withCredentials([usernamePassword(credentialsId: DOCKERHUB_CREDENTIALS_ID,
                                                             usernameVariable: 'DOCKERHUB_USER',
                                                             passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                                bat "docker login -u %DOCKERHUB_USER% --password %DOCKERHUB_PASSWORD%"
                                bat "docker push ${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}"
                            }
                        }
                    }
                }
    }
}