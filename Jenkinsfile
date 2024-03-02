
pipeline {
    
    agent any

    tools {
        nodejs 'nodejs16' 
    }

    stages {
        stage('Unit Test') {
            steps {
                sh 'npm --version'
                sh 'node --version'
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