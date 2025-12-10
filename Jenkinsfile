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
    }
}