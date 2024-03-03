
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

                    sh 'npm run test:coverage'

                    withSonarQubeEnv(installationName: 'sqserver') {
                        sh '''${sqscanner_home}/bin/sonar-scanner \
                            -D sonar.projectKey=myapp \
                            -D sonar.projectName=myapp \
                            -D sonar.sources=./src \
                            -D sonar.tests=./test \
                            -D sonar.javascript.lcov.reportPaths=./coverage/lcov.info
                        '''
                    }
                }
            }
        }

        stage('OWASP dependencies check') {
            steps {
                script {
                    dependencyCheck additionalArguments: ''' 
                        -o './'
                        -s './'
                        -f 'ALL' 
                        --prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
                    
                    dependencyCheckPublisher pattern: 'dependency-check-report.xml'
                }
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