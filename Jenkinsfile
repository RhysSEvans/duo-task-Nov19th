pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t rhyssevans/rhys-duo-jenk:latest -t rhyssevans/rhys-duo-jenk:v${BUILD_NUMBER} .
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push rhyssevans/rhys-duo-jenk:latest
                docker push rhyssevans/rhys-duo-jenk:v${BUILD_NUMBER}
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh'''
                kubectl apply -f .
                kubectl set image deployment/flask-deployment flask-container=rhyssevans/rhys-duo-jenk:v${BUILD_NUMBER}
                '''
            }
        }
    }
}
