
pipeline {
    
    agent any

    tools {
        nodejs 'nodejs16' 
    }

    environment {
        sqscanner_home = tool name: 'sqscanner5.0.1', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
    }

    stages {
        stage('Prepare') {
            steps {
                sh 'ls -la'
                sh 'node --version'
                sh 'npm --version'
                sh 'npm i --save-dev @types/node typescript jest @types/jest supertest'
                sh 'tsc --version'

                sh 'ls -la ${sqscanner_home}'
            }
        }

        stage('Unit Test') {
            steps {
                script {
                    echo 'Unit Test'

                    sh 'npm run test:ci'

                    withSonarQubeEnv(installationName: 'sqserver') {
                        sh '''${sqscanner_home}/bin/sonar-scanner \
                            -D sonar.projectKey=myapp \
                            -D sonar.projectName=myapp \
                            -D sonar.sources=./src \
                            -D sonar.tests=./test \
                            -D sonar.test.inclusions=./test/*.test.ts \
                            -D sonar.junit.reportPaths=./coverage
                        '''
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