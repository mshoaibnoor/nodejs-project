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
            
                  sh "docker build -t ${ImageName}:${imagetag} ." 
                  sh "docker scan ${ImageName}"
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
        
        stage('Apply Kubernetes files') {
            steps{
                withKubeConfig([credentialsId: 'jenkins-sa', serverUrl: 'https://kubernetes.docker.internal:6443']) {
                sh 'kubectl get deployment'
                }
            }
            
        }
        // stage('Deploy'){
        //     steps{
        //         // pre deployment steps
        //         sh '/usr/local/bin/kubectl get deployment'
        //         sh 'kubectl get service'
                
        //         // deployment steps
        //         sh "kubectl apply -f ${DeploymentName}"
        //         sh "kubectl apply -f ${ServiceName}"

        //         // post deployment steps
        //         sh 'kubectl get deployment'
        //         sh 'kubectl get service'
                
        //     }
        // }
    }

    // post{
    //     always{
    //         sh 'docker logout'
    //     }
    // }
}
