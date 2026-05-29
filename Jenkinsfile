pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                image 'node:18-alpine'
                resuedNode Tru
            }
            steps {
                sh '''
                ls -la
                node --version
                npm --version
                npm ci
                npm run build
                ls -la
                '''
            }
        }
    }
}