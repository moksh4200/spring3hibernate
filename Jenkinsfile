pipeline {
        agent any

        environment {
                registry = "653783293550.dkr.ecr.us-east-1.amazonaws.com/helm-demo"
        }

        stages {

                stage ('Build Image') {
                    steps {
                        script {
                           docker.build registry 

                        }
                    }
                }
                
                stage ('Push into ECR') {
                    steps {
                          sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 653783293550.dkr.ecr.us-east-1.amazonaws.com"
                          sh "docker push 653783293550.dkr.ecr.us-east-1.amazonaws.com/helm-demo:latest"
                    }
                }
        
             
                stage ('deploy to EKS using helm') {
                        kubeconfigId: 'K8S',
                    steps{

                       dir('kubernetes/') {
                           sh "helm install spring springapp/"
                       }
                    }
        }

                    
}

}