pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                    checkout scm
                }
            }
        }

        stage('Build FE') { 
            steps { 
                script{
                 frontend = docker.build("assigment-frontend", "-f Dockerfile src/frontend")
                }
            }
        }
        stage('Push') {
            steps {
                script{
                        docker.withRegistry('https://191627991384.dkr.ecr.ap-southeast-1.amazonaws.com/assigment-backend', 'ecr:ap-southeast-1:aws-credentials') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
        stage('Deploy'){
            steps {
                 sh 'kubectl apply -f deployment.yml'
            }
        }

    }
}