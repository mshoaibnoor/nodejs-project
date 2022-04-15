pipeline{
    agent any
    stages{
        // stage('Build'){
        //     steps{
        //         sh 'npm install'
        //     }
        // }
        stage('Docker Image Build'){
            steps{
               sh 'docker build -t main . -f Dockerfile.dockerfile'
               sh 'docker container run -d main -p 8100:8100'
            }

        }
        stage('Push to Docker Registry'){
            steps{
                sh 'docker tag nodejs mshoaibnoor/dev:nodejs'
                sh 'docker push mshoaibnoor/dev:nodejs'
            }
        }
    }
}