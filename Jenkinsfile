pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                echo 'Repository cloned successfully'
            }
        }

        stage('Build Backend') {
            steps {
                dir('ems-backend') {
                    sh 'chmod +x mvnw'
                    sh './mvnw clean package -DskipTests'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('ems-frontend') {
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                dir('ems-backend') {
                    sh 'docker build -t employee-backend .'
                }
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                dir('ems-frontend') {
                    sh 'docker build -t employee-frontend .'
                }
            }
        }

        stage('Deploy Backend') {
            steps {
                sh '''
                docker stop employee-backend-container || true
                docker rm employee-backend-container || true

                docker run -d \
                  --name employee-backend-container \
                  -p 8081:8081 \
                  employee-backend
                '''
            }
        }

        stage('Deploy Frontend') {
            steps {
                sh '''
                docker stop employee-frontend-container || true
                docker rm employee-frontend-container || true

                docker run -d \
                  --name employee-frontend-container \
                  -p 3000:80 \
                  employee-frontend
                '''
            }
        }
    }
}
