
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
                sh 'npm i --save-dev @types/node typescript'
                sh 'tsc --version'

                sh 'ls -la ${sqscanner_home}'
            }
        }

        stage('Unit Test') {
            steps {
                script {
                    echo 'Unit Test'
                    withSonarQubeEnv(installationName: 'sqserver') {
                        sh '''${sqscanner_home}/bin/sonar-scanner \
                            -D sonar.projectKey=YOUR_PROJECT_KEY_HERE \
                            -D sonar.projectName=YOUR_PROJECT_NAME_HERE \
                            -D sonar.projectVersion=YOUR_PROJECT_VERSION_HERE \
                            -D sonar.sources=./src \
                            -D sonar.test.inclusions=YOUR_INCLUSIONS_HERE \
                            -D sonar.exclusions=YOUR_EXCLUSIONS_HERE
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