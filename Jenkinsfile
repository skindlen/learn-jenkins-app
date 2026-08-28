pipeline {
    agent any

    stages {
        
        stage('Build') {
            agent {
               docker {
                image 'node:18-alpine'
                reuseNode true
               }
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

        stage('Tests') {
            parallel {
                stage('Test') {
                agent {
                    docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    test build/index.html

                    npm test
                '''    
            }
            post {
                always {
                    junit 'jest-results/junit.xml'
                }
            }
            
        }
        stage('Deploy') {
            agent {
               docker {
                image 'node:18-alpine'
                reuseNode true
               }
            }
            steps {
                sh '''
                npm install -g netlify-cli -g
                netlify --version
                '''
            }
        }

        
            }
        }

    }

}