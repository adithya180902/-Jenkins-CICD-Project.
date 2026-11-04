pipeline {

    agent any

    options {
        skipDefaultCheckout(true)
    }

    environment {
        PATH = "/Users/adithya/.docker/bin:/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
        IMAGE_NAME = "jenkins-cicd-project"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Fetching source code from GitHub'

                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker Image'

                sh 'docker build -t jenkins-cicd-project .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Docker Application'

                sh '''
                    docker rm -f jenkins-test 2>/dev/null || true

                    docker run -d \
                    --name jenkins-test \
                    -p 8081:80 \
                    jenkins-cicd-project

                    sleep 5

                    curl --fail http://localhost:8081

                    docker rm -f jenkins-test
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application'

                sh '''
                    docker rm -f jenkins-deployed 2>/dev/null || true

                    docker run -d \
                    --name jenkins-deployed \
                    -p 8082:80 \
                    jenkins-cicd-project
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline Completed Successfully!'
            echo 'Website: http://localhost:8082'
        }

        failure {
            echo 'CI/CD Pipeline Failed. Check the console output.'
        }
    }
}