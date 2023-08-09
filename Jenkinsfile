pipeline {
    agent any
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
        stage('Checkout chart repo') {
            steps {
                // Checkout code from your repository
                checkout scm
           }
        }
        
        stage('Push to GitHub') {
            steps {
                script {
                    def githubToken = credentials('github-token') // Use the ID of the credential you added
        sh '''
        git config --global user.email "kenneho@yahoo.com"
        git config --global user.name "nostradamuskenneh"
        rm -rf  CHARTS1
        git clone https://github.com/nostradamuskenneh/CHARTS1.git
        ls  
        cd CHARTS1
        cd weatherapp-weather/
        ls
        cat << EOF > dev-value.yaml
        replicaCount: 2
        image:
          repository: afakharany/weatherapp-weather${BUILD_NUMBER}
          tag: "${BUILD_NUMBER}"
        EOF

        ls
        cat << EOF > CHARTS1/weatherapp-ui/dev-value.yaml
        replicaCount: 2
        image:
          repository: afakharany/weatherapp-ui${BUILD_NUMBER}
          tag: "${BUILD_NUMBER}"
        EOF
        ls
        cat << EOF > CHARTS1/weatherapp-auth/dev-value.yaml
        replicaCount: 2
        image:
          repository: afakharany/weatherapp-auth{BUILD_NUMBER}
          tag: "${BUILD_NUMBER}"
        EOF
        ls
        cat << EOF > CHARTS1/weatherapp-mysql/dev-value.yaml
        replicaCount: 2
        image:
          repository: afakharany/weatherapp-mysql${BUILD_NUMBER}
          tag: "${BUILD_NUMBER}"
        EOF
        ls
        cd CHARTS1
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

        stage('Update File') {
            steps {
                script {
                    def githubToken = credentials('github-token') // Use the ID of the GitHub token credential
                    
                    def repoOwner = 'nostradamuskenneh'
                    def repoName = 'CHARTS1'
                    def filePath = 'weatherapp-weather/dev-value.yaml'
                    def commitMessage = 'Update file via Jenkins'
                    def newFileContent = "${BUILD_NUMBER}"
                    
                    def apiUrl = "https://github.com/nostradamuskenneh/CHARTS1.git"
                    
                    def response = httpRequest(
                        authentication: githubToken,
                        contentType: 'APPLICATION_JSON',
                        httpMode: 'PATCH',
                        url: apiUrl,
                        requestBody: [
                            message: commitMessage,
                            content: newFileContent.encodeBase64(),
                            sha: 'SHA-of-the-existing-file'
                        ]
                    )
                    
                    echo "File updated with response: ${response}"
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
    }

}
