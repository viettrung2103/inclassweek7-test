pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS_ID = 'Docker_Hub'
        DOCKERHUB_REPO = 'cindy3377/inclassweek7'
        DOCKER_IMAGE_TAG = 'latest_v1'
    }
    stages {
        stage('Install Maven') {
            steps {
                sh '''
                # Check if Maven is installed
                if ! command -v mvn &> /dev/null
                then
                    echo "Maven not found, installing..."
                    sudo apt update && sudo apt install -y maven
                else
                    echo "Maven is already installed."
                fi
                mvn -version
                '''
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
                script {
                    docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
                }
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                        docker.image("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
