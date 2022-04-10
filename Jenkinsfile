pipeline{
    agent any
    stages{
        stage('Build'){
            steps{
                sh 'npm install'
            }
        }
        stage('Docker Image Build'){
            steps{
               sh 'docker build -t main . -f Dockerfile.dockerfile'
               sh 'docker container run -d main'
            }

        }
    }
}