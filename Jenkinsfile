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
                    ls --all --human-readable --literal

                    node --version
                    npm --version

                    npm clean-install
                    npm run build

                    ls --all --human-readable --literal
                '''
            }
        }
    }
}
