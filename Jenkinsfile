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
               sh 'Docker build -t main . -f Dockerfile.dockerfile'
            }
        }
    }
}