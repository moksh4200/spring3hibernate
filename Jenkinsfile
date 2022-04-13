pipeline {
        agent any

        environment {
                registry = "653783293550.dkr.ecr.us-east-1.amazonaws.com/helm-demo"
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
                          sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 653783293550.dkr.ecr.us-east-1.amazonaws.com"
                          sh "docker push ${registry}:V${BUILD_NUMBER}"
                    }
                }
        
                /* 
                stage ('deploy to EKS using helm') {
                    
                    steps {
                      kubernetesDeploy (
                        configs: 'helm-local/',
                        kubeconfigId: 'K8S',
                    )
                         sh 'helm install  --set appimage=${registry}:V${BUILD_NUMBER} spring helm-local/'
                       }
                    }
                */

                stage('Kubernetes Deploy') {
            steps {
                container('helm') {
                    sh "helm upgrade --install --force --set name=${NAME} --set image.tag=${VERSION} --set domain=${DOMAIN} ${NAME} ./helm"
                }
            }
        }
            
        }              
}

