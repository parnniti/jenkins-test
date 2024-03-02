
pipeline {
    
    agent any

    tools {
        nodejs 'nodejs16' 
    }

    stages {
        stage('Prepare') {
            steps {
                sh 'ls -la'
                sh 'node --version'
                sh 'npm --version'
                sh 'npm i --save-dev @types/node typescript'
                sh 'tsc --version'
            }
        }

        stage('Unit Test') {
            steps {
                script {
                    withSonarQubeEnv(installationName: 'sqserver') {
                        echo 'Hello World!'
                    }
                }
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