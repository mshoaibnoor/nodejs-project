def ImageName = "mshoaibnoor/dev"
def imagetag = "nodejs-latest"
def DeploymentName = "nodejs-deployment.yaml"
def ServiceName = "nodejs-service.yaml"

pipeline{
    agent any
    environment{
        DOCKERHUB_CREDENTIALS = credentials('mshoaibnoor-dockerhub')
    }
    stages{
        // stage('Build'){
        //     steps{
        //         sh 'npm install'
        //     }
        // }
        stage('Docker Image Build'){
            steps{
            
                  sh "sudo docker build -t ${ImageName}:${imagetag} ." 
            }

        }
        stage('Login'){
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push to Docker Registry'){
            steps{
                sh "docker push ${ImageName}:${imagetag}"
            }
        }
        stage('Deploy'){
            steps{
                // pre deployment steps
                sh 'kubectl get deployment'
                sh 'kubectl get service'
                
                // deployment steps
                sh "kubectl apply -f ${DeploymentName}"
                sh "kubectl apply -f ${ServiceName}"

                // post deployment steps
                sh 'kubectl get deployment'
                sh 'kubectl get service'
            }
        }
    }
    // post{
    //     always{
    //         sh 'docker logout'
    //     }
    // }
}
