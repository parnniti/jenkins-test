
pipeline {
    
    agent any

    tools {
        nodejs 'nodejs16' 
    }

    stages {
        stage('Unit Test') {
            steps {
                sh 'ls -la'
                sh 'npm --version'
                sh 'node --version'

                sh 'npm run test:ci'
                echo 'Hello World!'
            }
        }

        stage('OWASP dependencies check') {
            steps {
                echo 'OWASP'
            }
        }

        stage('Build') {
            steps {
                echo 'Build'
            }
        }

        stage('Deployment') {
            steps {
                echo 'Deployment'
            }
        }
    }
}