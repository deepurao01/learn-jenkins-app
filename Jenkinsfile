pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -ls
                    node --version
                    npm --version
                    # it is degined for CI to install the dependency
                    npm ci  
                    npm run build
                    ls -ls

                '''
            }
        }
        stage('Test'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                script {
                    if (fileExists('build/index.html')){
                        echo "index file found ! now going to run test"
                        sh 'npm test'

                    } else {
                        echo "Index file doesn't exist "
                    }
                }
            }

        }
        stage('E2E'){
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.57.0-noble'
                    reuseNode true
                }
            }
            steps{
                sh '''
                /*
                npm install serve 
                # run serve as root and & means running in background 
                node_modules/.bin/serve -s build &
                sleep 10
                npx playwright test --reporter=html
                */
                '''
            }

        }
        stage('Deploy'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                  npm install netlify-cli
                  netlify --version

                '''
            }
        }
        
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}