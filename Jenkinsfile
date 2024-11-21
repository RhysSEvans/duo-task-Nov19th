pipeline {
    agent any
    stages {
        stage('Init') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh'''
                        kubectl create ns prod || echo "----- prod namespace already exists -----"
                        '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh'''
                        kubectl create ns dev || echo "----- dev namespace already exists -----"
                        '''
                    } else {
                        sh'echo "unrecognised branch"'
                    }
                }
            }
        }
        stage('Build') {
            steps {
                 script {
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh'''
                        docker build -t rhyssevans/rhys-duo-jenk:latest -t rhyssevans/rhys-duo-jenk:v${BUILD_NUMBER} .
                        '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh'''
                        docker build -t rhyssevans/rhys-duo-jenk:latest -t rhyssevans/rhys-duo-jenk:v${BUILD_NUMBER} .
                        '''
                    } else {
                        sh'echo "unrecognised branch"'
                    }
                }
            }
        }


        stage('Push') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh'''
                        docker push rhyssevans/rhys-duo-jenk:latest
                        docker push rhyssevans/rhys-duo-jenk:v${BUILD_NUMBER}
                        '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh'''
                        docker push rhyssevans/rhys-duo-jenk:latest
                        docker push rhyssevans/rhys-duo-jenk:v${BUILD_NUMBER}
                        '''
                    } else {
                        sh'echo "unrecognised branch"'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/main') {
                        sh'''
                        kubectl apply -f . -n dev
                        kubectl set image deployment/flask-deployment flask-container=rhyssevans/rhys-duo-jenk:v${BUILD_NUMBER} -n dev
                        '''
                    } else if (env.GIT_BRANCH == 'origin/dev') {
                        sh'''
                        kubectl apply -f . -n dev
                        kubectl set image deployment/flask-deployment flask-container=rhyssevans/rhys-duo-jenk:v${BUILD_NUMBER} -n dev
                        '''
                    } else {
                        sh'echo "unrecognised branch"'
                    }
                }
            }
        }
    }
}
