
        
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

       stage('scan trivy'){
        steps {
          bat 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest my-php-app-api -o table > trivy_table_output_new.txt '     
       }
       }
    }
    
   
}
