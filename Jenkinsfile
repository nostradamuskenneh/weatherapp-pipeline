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
echo
       " replicaCount: {{ .Values.replicaCount }} 
        
        image:
          repository: {{ .Values.image.repository }}
          pullPolicy: {{ .Values.image.pullPolicy }}
          tag: {{ .Values.image.tag | default "" }}
        
        imagePullSecrets: {{ toYaml .Values.imagePullSecrets | nindent 2 }}
        
        nameOverride: {{ .Values.nameOverride | default "" }}
        fullnameOverride: {{ .Values.fullnameOverride | default "" }}
        
        serviceAccount:
          create: {{ .Values.serviceAccount.create }}
          annotations: {{ toYaml .Values.serviceAccount.annotations | nindent 4 }}
          name: {{ .Values.serviceAccount.name | default "" }}
        
        podAnnotations: {{ toYaml .Values.podAnnotations | nindent 2 }}
        
        podSecurityContext: {{ toYaml .Values.podSecurityContext | nindent 2 }}
        
        securityContext: {{ toYaml .Values.securityContext | nindent 2 }}
        
        service:
          type: {{ .Values.service.type }}
          port: {{ .Values.service.port }}
        
        ingress:
          enabled: {{ .Values.ingress.enabled }}
          className: {{ .Values.ingress.className | default "" }}
          annotations: {{ toYaml .Values.ingress.annotations | nindent 4 }}
          hosts:
        {{- range .Values.ingress.hosts }}
            - host: {{ .host | default "chart-example.local" }}
              paths:
        {{- range .paths }}
                - path: {{ .path | default "/" }}
                  pathType: {{ .pathType | default "ImplementationSpecific" }}
        {{- end }}
        {{- end }}
          tls: []
        
        resources: {{ toYaml .Values.resources | nindent 2 }}
        
        autoscaling:
          enabled: {{ .Values.autoscaling.enabled }}
          minReplicas: {{ .Values.autoscaling.minReplicas }}
          maxReplicas: {{ .Values.autoscaling.maxReplicas }}
          targetCPUUtilizationPercentage: {{ .Values.autoscaling.targetCPUUtilizationPercentage }}
        
        nodeSelector: {{ toYaml .Values.nodeSelector | nindent 2 }}
        
        tolerations: {{ toYaml .Values.tolerations | nindent 2 }}
        
        affinity: {{ toYaml .Values.affinity | nindent 2 }} " > weatherapp-auth/dev-value.yaml
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
    //}
    //}


   // post {
   //     always {
   //         // Send a Slack notification after the build completes (success or failure)
   //         script {
   //             slackSend(
   //                 color: currentBuild.resultIsBetterOrEqualTo('SUCCESS') ? 'good' : 'danger',
   //                 message: "Build Status: ${currentBuild.currentResult} \nJob Name: ${env.JOB_NAME} \nBuild Number: ${env.BUILD_NUMBER}",
   //                 channel: '#dev-lions',
   //                 tokenCredentialId: 'slack-jenkins-token-ID'
   //             )
   //         }
       }
    }
