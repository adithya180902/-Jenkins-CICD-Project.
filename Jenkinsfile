pipeline {
    agent any

    environment {
        PATH = "/Users/adithya/.docker/bin:/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    }

    stages {

        stage('Prepare') {
            steps {
                echo 'Creating Website Files'

                writeFile file: 'index.html', text: '''
                <!DOCTYPE html>
                <html>
                <head>
                    <title>Jenkins CI/CD Demo</title>
                </head>
                <body>
                    <h1>Welcome to Jenkins CI/CD</h1>
                    <p>Application built and tested using Docker.</p>
                </body>
                </html>
                '''

                writeFile file: 'Dockerfile', text: '''
                FROM nginx:alpine
                COPY index.html /usr/share/nginx/html/index.html
                EXPOSE 80
                '''
            }
        }

        stage('Build') {
            steps {
                echo 'Building Docker Image'

                sh 'docker build -t jenkins-demo .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing Application'

                sh '''
                    docker rm -f test-container 2>/dev/null || true

                    docker run -d --name test-container -p 8081:80 jenkins-demo

                    sleep 3

                    curl --fail http://localhost:8081

                    docker rm -f test-container
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Application'

                sh '''
                    docker rm -f jenkins-deployed 2>/dev/null || true

                    docker run -d --name jenkins-deployed -p 8082:80 jenkins-demo
                '''
            }
        }
    }
}