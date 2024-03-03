
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
                sh '''
                    node --version
                    npm --version
                    tsc --version
                '''
                sh 'npm install'
            }
        }

        stage('Unit Test') {
            steps {
                script {
                    sh 'npm run test:ci'
                    sh 'npm run test:coverage'
                    sh 'ls -la ./coverage'

                    junit '**/coverage/junit.xml'

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
                        --prettyPrint''', odcInstallation: 'depcheck9.0.9'
                    
                    dependencyCheckPublisher pattern: 'dependency-check-report.xml'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    withDockerRegistry([url: 'https://index.docker.io/v1/', credentialsId: 'dockerhub']) {
                        String container_tag = 'pxrn/myapp:v1'
                        sh "docker build --no-cache -f ./Dockerfile -t $container_tag ."
                        sh "docker push $container_tag"
                    }
                }
            }
        }

        stage('Deployment') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/parnniti/myapp-chart.git'
                    ]]
                )

                sh 'ls -la'
            }
        }
    }

    post {
        always {
            sh 'rm -rf node_modules/ package-lock.json'
        }
    }
}