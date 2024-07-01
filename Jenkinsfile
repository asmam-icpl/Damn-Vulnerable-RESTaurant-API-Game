pipeline {
    agent any

    stages {
        stage('SCM Checkout') {
            steps {
                checkout scm
            }
        }
        
  // stage('SonarQube Analysis') {
  //   def scannerHome = tool 'SonarScanner';
  //   withSonarQubeEnv() {
  //     bat "${scannerHome}/bin/sonar-scanner"
  //   }
  // }

    stage('trivy scan') {
    steps {
        script {
            bat 'docker pull aquasec/trivy'
            bat 'docker build -t my-php-app .'
            // Ensure the output directory exists
            
            //bat "docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image -f table my-php-app:latest > trivy_table_output.txt"
            bat """
            docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latest image --format template --template '@contrib/html.tpl' -o /trivy_output.html my-php-app:latest
"""
        }
    }
    }
}
}
