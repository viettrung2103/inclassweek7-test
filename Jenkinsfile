pipeline {
    agent any
     environment {
            // Define Docker Hub credentials ID
            DOCKERHUB_CREDENTIALS_ID = 'Docker_Hub'
            // Define Docker Hub repository name
            //CHAN GE THIS ACCORDING TO YOUR GITHUB LINK IN DOCKER
            DOCKERHUB_REPO = 'cindy3377/inclassweek7'
            // Define Docker image tag
            DOCKER_IMAGE_TAG = 'latest_v1'
        }
    stages {
        stage('Build') {
                steps {
                    sh '/path/to/maven/bin/mvn clean install'
                }
            }
        stage('Checkout') {
            steps {
                git 'https://github.com/cindy3377/inclassweek7.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Coverage') {
            steps {
                sh 'mvn jacoco:report'
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
                            docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
                        }
                    }
                }
                stage('Push Docker Image to Docker Hub') {
                    steps {
                        // Push Docker image to Docker Hub
                        script {
                            docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                                docker.image("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                            }
                        }
                    }
                }
    }
}