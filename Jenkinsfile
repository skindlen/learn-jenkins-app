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

        stage('E2E') {
            agent {
               docker {
                image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                reuseNode true
               }
            }
            steps {
                sh '''
                    npm install serve
                    npx serve -s build -l 3000 &
                    sleep 10
                    npx playwright test --reporter=html
                '''    
            }
            post {
                always {
                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwrite HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                }
            }
            
        } 
            }
        }

    }

}