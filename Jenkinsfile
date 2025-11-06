pipeline {
    agent any

    stages {
        // this is a comment
        /*
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
        */
        /*
          we are write comments for 
           test
        */
        stage('Test'){
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('E2E'){
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install  serve
                    node_module/.bin/serve -s build
                    npx playwright test
                '''
            }
        }
    }
    post {
        always {
            junit 'test-result/junit.xml'
        }
    }
}