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
                docker   tag db oumarkenneh/db
                docker   tag ui oumarkenneh/ui
                docker   tag auth oumarkenneh/auth
                docker   tag weather oumarkenneh/weather
                ls 
                pwd
                '''
            }
        }
        stage('publish images') {
            steps {
                sh '''
                docker push  oumarkenneh/db
                docker push  oumarkenneh/ui
                docker push  oumarkenneh/auth
                docker push  oumarkenneh/weather
                ls 
                pwd
                '''
            }
        }
        stage('Checkout chart repo') {
            steps {
                // Checkout code from your  repository
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
         
        cat << EOF > CHARTS1/weatherapp-weather/dev-value.yaml
        replicaCount: 2
        image:
          repository: afakharany/weatherapp-weather
          tag: ""
        EOF        

        ls
        cat << EOF > CHARTS1/weatherapp-ui/dev-value.yaml
        replicaCount: 2
        image:
        repository: afakharany/weatherapp-ui
        tag: ""
        EOF

        ls
        cd CHARTS1
        ls
        cat << EOF > CHARTS1/weatherapp-auth/dev-value.yaml
        replicaCount: 2
        image:
        repository: afakharany/weatherapp-auth
        tag: ""
        EOF

        ls
        cd CHARTS1
        ls
        cat << EOF > CHARTS1/weatherapp-mysql/dev-value.yaml
        replicaCount: 2
        image:
        repository: afakharany/weatherapp-mysql
        tag: ""
        EOF
        git add .
        git commit -m "Jenkins automated commit"
        git push origin main
                    '''
                }
            }
        }
        stage('update weatherapp-weather') {
            steps {
                sh '''
                ls
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
