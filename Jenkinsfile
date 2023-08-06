pipeline {
    agent any

    stages {
       stage('Login') {
   
	   		steps {
	   			sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
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