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
    }
}
