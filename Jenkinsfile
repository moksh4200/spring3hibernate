pipeline {
        agent any

        environment {
                registry = "666697415673.dkr.ecr.us-east-1.amazonaws.com/spring"
        }

        stages {

                stage ('Build Image') {
                    steps {
                        script {
                           dockerImage = docker.build registry + ":V$BUILD_NUMBER" 

                        }
                    }
                }
                
                stage ('Push into ECR') {
                    steps {
                          sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 666697415673.dkr.ecr.us-east-1.amazonaws.com"
                          sh "docker push ${registry}:V${BUILD_NUMBER}"
                    }
                }
        
                
                stage ('deploy to EKS using helm') {
                    
                     
                    
                    steps {
                         
                           sh 'helm upgrade --install --set image=${registry}:V${BUILD_NUMBER} spring helm-local/'
                    
                        }
                }
            
            }

        }


