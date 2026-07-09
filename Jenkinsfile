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
                    sh 'chmod +x mvnw || true'
                    sh './mvnw clean package -DskipTests || mvn clean package -DskipTests'
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
    }
}
