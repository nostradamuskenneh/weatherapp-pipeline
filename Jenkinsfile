pipeline {
    agent any
    stages {
      stage('Login to Docker Hub') {
        steps{
          sh 'echo Amara1988 | docker login -u oumarkenneh --password-stdin'
          echo 'Login Completed'
        }
      }
      stage('clone') {
         steps {
             sh '''
             ls 
             pwd
             '''
         }
     }
      stage('SonarQube analysis') {
         agent {
             docker {
               image 'sonarsource/sonar-scanner-cli:4.7.0'
             }
            }
            environment {
     CI = 'true'
     //  scannerHome = tool 'Sonar'
     scannerHome='/opt/sonar-scanner'
 }
         steps{
             withSonarQubeEnv('Sonar') {
                 sh "${scannerHome}/bin/sonar-scanner"
             }
                  }
              }

           stage('Login to Docker Hub') {
            steps{
              sh 'echo Amara1988 | docker login -u oumarkenneh --password-stdin'
              echo 'Login Completed'
            }
          }
      stage('clone') {
         steps {
                sh '''
                ls 
                pwd
                '''
            }
        }

 //  stage('SonarQube Analysis') {
 //   def scannerHome = tool 'SonarScanner';
 //   withSonarQubeEnv() {
 //     sh "${scannerHome}/bin/sonar-scanner"
 //   }
 // }
        stage('build') {
            steps {
                sh '''
                ls 
                pwd
                '''
            }
        }
    }
}

