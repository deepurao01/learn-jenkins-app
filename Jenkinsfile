pipeline {
    agent any

    stages {
        stage('Docker'){
            steps{
                sh 'docker build -t my_playwright .'
            }
        }
        stage('Hello') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
