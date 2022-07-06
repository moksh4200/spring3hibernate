pipeline {
        agent any

        environment {
                registry = "043394070357.dkr.ecr.ap-south-1.amazonaws.com/spring-repo"
        }
        /* 
        tools {
                    maven "mvn3.3"
                    jdk "jdk8"
            }
        */


        stages {
        /* 
            stage('Maven Build') {
                            steps {
                                sh 'mvn clean install'
                                sh 'mvn package'
                        }
                    }
        */            
                stage ('Build Image') {
                    steps {
                        script {
                           dockerImage = docker.build registry + ":V$BUILD_NUMBER" 

                        }
                    }
                }
                
                stage ('Push into ECR') {
                    steps {
                          sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 043394070357.dkr.ecr.ap-south-1.amazonaws.com"
                          sh "docker push ${registry}:V${BUILD_NUMBER}"
                    }
                }
                

                 
            /*
                
                stage ('deploy to EKS using helm') {
                    
                     
                    
                    steps {
                         
                           sh 'helm upgrade --install --set image=${registry}:V${BUILD_NUMBER} spring helm-local/'
                    
                        }
                }
            
            */
            }


        }


