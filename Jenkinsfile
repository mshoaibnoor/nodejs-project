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
        
        kubeconfig(caCertificate: '''MIIC/jCCAeagAwIBAgIBADANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwprdWJl
            cm5ldGVzMB4XDTIyMDMwNDEyNDkzOVoXDTMyMDMwMTEyNDkzOVowFTETMBEGA1UE
            AxMKa3ViZXJuZXRlczCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMMR
            9cwteiARgpaFKPwN7gVkqmeGb33nuosMOzSka50+p0nc5AvAhwPFhc+ObxK+6XL2
            xjO73/AGb/n3nT+uUtwUIlRpHgQrbCjfAJ4IIQo3WZRA+GAxXhZxZjebr0catcTG
            CciVG+tq4v6v3dQm8cWa0ACV71xjZJdpB3KBRXm+rN7Cdu0DsIcd8BMMW2VBUvvG
            ruArGLOOs/OFD7wKRPoCD0FxOX7FD9qx+BvR1/xV/9a2ojTl1d/oc18mPRQtchzl
            mlnCPzG3l/cg+gsMrALBoLZOkabWyWY+a/hZImsO2A/cYxSVQHCgvfrylupmrbRV
            LpOGczQV7rBnckyZVSkCAwEAAaNZMFcwDgYDVR0PAQH/BAQDAgKkMA8GA1UdEwEB
            /wQFMAMBAf8wHQYDVR0OBBYEFOev9CfPAYw4tiPyREExSNCIiSqFMBUGA1UdEQQO
            MAyCCmt1YmVybmV0ZXMwDQYJKoZIhvcNAQELBQADggEBAD/+6xYY/p66pbe8BcUU
            617bIwUNDblG4RfYgiEyt3vp/SWfmPSx3GGt8OkT2B6Wyh1Wr5v0dSkE8VIn5tu+
            t1qfMduFwPdLCqOXxPCvTRecbaBOAX/FZVDN6B0PQo+L9wg8sXIcVtVpwwmkZhae
            +JPO6hbXFLONRyLx0fScpe4nXNC9w39oofbPtaVyXSS7q2wOfJ5vNaTEo0LhOoST
            9n1ihTrJq437+RS8x3qwgq9UqPsXy7a5BRxZdoN3idcYbrZTLzTHPzA/HSxCudlJ
            h/DIXZiFmVH8xSzE/H2NI5imGJB+16QSmZJ2BI0SZE99uRTON0xFEUXoE2mlKxMk
            EVE=''', serverUrl: 'https://kubernetes.docker.internal:6443') {
                sh 'kubectl get deployment'
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
