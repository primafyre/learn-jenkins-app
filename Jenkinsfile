pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -ahl

                    node --version
                    npm --version

                    npm clean-install
                    npm run build

                    ls -ahl
                '''
            }
        }

        stage('Tests') {
            parallel {
                stage('Unit tests') {
                    agent {
                        docker {
                            image 'node:22-alpine'
                            reuseNode true
                        }
                    }
                    steps{
                        sh '''
                            test -f build/index.html

                            npm test
                        '''
                    }
                    post {
                        always {
                            junit 'test-results/junit.xml'
                        }
                    }
                }

                stage('E2E tests') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.50.1-noble'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                            npm install serve
                            node_modules/.bin/serve -s build &
                            sleep 10
                            npx playwright test --reporter=html

                            ls -ahl
                        '''
                    }
                    post {
                        always {
                            publishHTML(target: [allowMissing: false, alwaysLinkToLastBuild: false, keepAll: true, reportDir: 'test-results', reportFiles: 'index.html', reportName: 'E2E Tests', reportTitles: ''])
                        }
                    }
                }
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli -g
                    netlify --version
                '''
            }
        }
    }
}
