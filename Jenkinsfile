pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t amckee457/flask-app-kubernetes:latest -t amckee457/flask-app-kubernetes:v${BUILD_NUMBER} .
                docker build -t amckee457/db-kubernetes:latest -t amckee457/db-kubernetes:v${BUILD_NUMBER} .
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push amckee457/flask-app-kubernetes:latest
                docker push amckee457/flask-app-kubernetes:v${BUILD_NUMBER}
                docker push amckee457/db-kubernetes:latest
                docker push amckee457/db-kubernetes:v${BUILD_NUMBER}
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh'''
                kubectl apply -f ./k8s
                kubectl set image deployment/flask-deployment-exercise flask-container-exercise=amckee457/flask-app-kubernetes:v${BUILD_NUMBER}
                kubectl set image pod/mysql mysql-container=amckee457/flask-app-kubernetes:v${BUILD_NUMBER}
                '''
            }
        }
    }
}
