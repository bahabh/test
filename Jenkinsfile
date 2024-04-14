pipeline {
   agent any
  stages {
    stage('Setting up OWASP ZAP docker container') {
             steps {
                 script {
                         echo "Pulling up last OWASP ZAP container --> Start"
                         sh 'docker pull iyedbnaissa/phpstan:29'
                         echo "Pulling up last VMS container --> End"
                         echo "Starting container --> Start"
                         sh """
                         docker run -dt --name owasp \
                         owasp/zap2docker-stable \
                         /bin/bash
                         """
                 }
             }
         }
   stage(phpstan_scan){
      steps{
        container('phpstan'){
          script{
            sh "mkdir -p /tmp/phpstan_cache"
            sh "chmod 755 /tmp/phpstan_cache"
            // Create directory for reports
            sh "mkdir -p test-reports"
            // Run PHPStan inside PHPStan container
            sh "phpstan -vvv analyse --error-format=json -a build/phpstan/bootstrap_action.php > test-reports/phpstan-report.json"
          }
        }
      }
    }
    stage('SonarQube Analysis') {
      environment {
             SONAR_SCANNER_OPTS = "-Xmx512m"
      }
      steps {
        script {
          def scannerHome = tool 'SonarQube_Scanner';
          // Execute SonarQube analysis
          withSonarQubeEnv('SonarQube_Server') {
           
            sh "${scannerHome}/bin/sonar-scanner -Dproject.settings=sonar-project.properties"
          }
        }
      }
    }
    
  }
}
