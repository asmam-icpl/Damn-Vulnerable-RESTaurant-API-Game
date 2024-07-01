
        
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

       // stage('scan trivy'){
       //  steps {
       //    bat 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy:latestimage -f table my-php-app-api:latest > trivy_table_output_new.txt '     
       // }
       // }
        stage('syft scan')
        {
        steps{
           script{
                bat  " docker run --rm -v /var/run/docker.sock:/var/run/docker.sock anchore/syft:latest my-php-app-api -o table > syft_output.txt"
           }
        }
        }

     
     
    stage('syft report'){
           steps{
                echo "${env.JENKINS_URL}job/${env.JOB_NAME}/${env.BUILD_NUMBER}/execution/node/3/ws/syft_output.txt"

            
           }
     }
    stage('gryp scan')
        {
        steps{
           script{
             bat  " docker run --rm -v /var/run/docker.sock:/var/run/docker.sock anchore/grype:latest my-php-app-api -o table "
           }
        }
        }

       stage('gryp report'){
           steps{
                echo "${env.JENKINS_URL}job/${env.JOB_NAME}/${env.BUILD_NUMBER}/execution/node/3/ws/gryp_output.txt"

    }
     
    }
    }
    
   
}
