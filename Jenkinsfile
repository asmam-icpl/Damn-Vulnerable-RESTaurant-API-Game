pipeline {
    agent any

    stages {
        stage('Pull Trivy Image') {
            steps {
                script {
                    echo 'Pulling Trivy image...'
                    bat 'docker pull aquasec/trivy'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    bat 'docker build -t my-php-app-api .'
                }
            }
        }

        stage('Trivy Scan') {
            steps {
                script {
                    echo 'Running Trivy scan...'
                    //bat 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image -f table my-php-app-api:latest > trivy_output.txt'
                    bat 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image -f table my-php-app-api:latest > abc_output.txt'
                }
            }
        }

        stage('Archive Trivy Scan Result') {
            steps {
                archiveArtifacts artifacts: 'trivy_output.txt', allowEmptyArchive: true
            }
        }
    }
}
