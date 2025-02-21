pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'b6779168-b863-472e-acc6-6c2028509339'
        NETLIFY_AUTH_TOKEN = credentials('NETLIFY_PAT')
    }

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

                    netlify status

                    echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                '''
            }
        }
    }
}
