pipeline {
    agent any

    /*
    lol
    */
    
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
                    npm ci
                    npm run build
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                  test -f build/index.html
                  npm run test
                '''
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
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}