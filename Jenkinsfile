def ImageName = "mshoaibnoor/dev"
def imagetag = "nodejs-latest"

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
                sh 'kubectl apply -f nodejs-deployment.yaml'
                sh 'kubectl apply -f nodejs-service.yaml'
            }
        }
    }
    post{
        always{
            sh 'docker logout'
        }
    }
}