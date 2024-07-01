
        
  // stage('SonarQube Analysis') {
  //   def scannerHome = tool 'SonarScanner';
  //   withSonarQubeEnv() {
  //     bat "${scannerHome}/bin/sonar-scanner"
  //   }
  // }

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
                    def scanResult = bat(script: 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image -f json my-php-app-api:latest', returnStdout: true).trim()
                    writeFile file: 'trivy-result.json', text: scanResult

                    def scanData = readJSON file: 'trivy-result.json'
                    def criticalCount = scanData.Results[0].Vulnerabilities.findAll { it.Severity == 'CRITICAL' }.size()

                    echo "Total vulnerabilities: ${scanData.Results[0].Vulnerabilities.size()}"
                    echo "Critical vulnerabilities: ${criticalCount}"

                    if (criticalCount > 0) {
                        error("Critical vulnerabilities found: ${criticalCount}")
                    }
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'trivy-result.json', allowEmptyArchive: true
        }

        failure {
            echo 'Pipeline failed due to critical vulnerabilities.'
        }
    }
}
