pipeline {
    agent any
    environment{
        NETLIFY_SITE_ID = '2cd6a58d-1a12-45a6-8342-43d0ac265666'
    }
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
                npm install serve 
                # run serve as root and & means running in background 
                node_modules/.bin/serve -s build &
                
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
                  npm install netlify-cli@20.1.1
                  netlify --version
                  echo "Deploying to production. Site ID : $NETLIFY_SITE_ID"
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