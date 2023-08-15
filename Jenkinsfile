pipeline {
    agent {
        label 'node'
    }
    stages {
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
      stage('Build DB') {
         steps {
                sh '''
                cd DB
                docker build -t db .
                cd -
                cd UI
                docker build -t ui .
                cd -
                cd auth
                docker build -t auth .
                cd -
                cd weather
                docker build -t weather .
                
                
                ls 
                pwd
                '''
            }
        }

        stage('  tag images') {
            steps {
                sh '''
                docker   tag db oumarkenneh/db:${BUILD_NUMBER}
                docker   tag ui oumarkenneh/ui:${BUILD_NUMBER}
                docker   tag auth oumarkenneh/auth:${BUILD_NUMBER}
                docker   tag weather oumarkenneh/weather:${BUILD_NUMBER}
                ls 
                pwd
                '''
            }
        }
        stage('publish images') {
            steps {
                sh '''
                docker push  oumarkenneh/db:${BUILD_NUMBER}
                docker push  oumarkenneh/ui:${BUILD_NUMBER}
                docker push  oumarkenneh/auth:${BUILD_NUMBER}
                docker push  oumarkenneh/weather:${BUILD_NUMBER}
                ls 
                pwd
                '''
            }
       }
        stage('Build') {
            steps {
                script {
                    sh '''
                        rm -rf  CHARTS1 || true
                        git clone git@github.com:nostradamuskenneh/CHARTS1.git
                        cd CHARTS1
                        echo "image: 
                                repository: oumarkenneh/auth
                                tag:${BUILD_NUMBER}" >  weatherapp-auth/dev-value.yaml

                        echo "image: 
                                repository: oumarkenneh/db
                                tag:${BUILD_NUMBER}" > weatherapp-mysql/dev-value.yaml
                        echo "image: 
                                repository: oumarkenneh/weather
                                tag:${BUILD_NUMBER}" > weatherapp-weather/dev-value.yaml
                        echo "image: 
                                repository: oumarkenneh/ui
                                tag:${BUILD_NUMBER}" > weatherapp-ui/dev-value.yaml
                        git add .
                        git commit -m "Jenkins automated commit"
                        git push origin main
                        ls
                        pwd
                        id

                    '''
                }
            }
        }

        stage('update weatherapp-weather') {
            steps {
                sh '''
                ls
                pwd
                '''
            }
        }
        stage('update weatherapp-ui') {
            steps {
                sh '''
                ls
                '''
            }
        }
        stage('update weatherapp-auth') {
            steps {
                sh '''
                 pwd

                '''
            }
        }
        stage('update weatherapp-mysql') {
            steps {
        sh '''
           id

        '''
            }
        }
    



     post {
        always {
            // Send a Slack notification after the build completes (success or failure)
            script {
                slackSend(
                    color: currentBuild.resultIsBetterOrEqualTo('SUCCESS') ? 'good' : 'danger',
                    message: "Build Status: ${currentBuild.currentResult} \nJob Name: ${env.JOB_NAME} \nBuild Number: ${env.BUILD_NUMBER}",
                    channel: '#dev-lions',
                    tokenCredentialId: 'slack-jenkins-token-ID'
                )
            }
       }
    }
  }
}
