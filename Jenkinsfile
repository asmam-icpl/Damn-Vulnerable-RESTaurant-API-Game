node {
  stages{
  stage('SCM') {
    checkout scm
  }
  // stage('SonarQube Analysis') {
  //   def scannerHome = tool 'SonarScanner';
  //   withSonarQubeEnv() {
  //     bat "${scannerHome}/bin/sonar-scanner"
  //   }
  // }

    stage('trivy scan') {
    steps {
       def outputFilePath = "${env.WORKSPACE}"
        bat 'docker pull aquasec/trivy'
        bat 'docker build -t my-php-app .'
        //bat "docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v ${WORKSPACE}:/root/.cache/ aquasec/trivy image --format json --output /root/.cache/trivy-report.json my-php-app"
        bat 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v C:\Users\AsmaM\trrufellhog_project\Damn-Vulnerable-RESTaurant-API-Game:/output aquasec/trivy:latest image --format table -o /output/report-trivy.txt my-php-app:latest'
    }
}
}
}
