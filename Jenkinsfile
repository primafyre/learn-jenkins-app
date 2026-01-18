pipeline {
	agent any

	stages {
		stage('build') {
			agent {
				docker {
					image 'node:24.13.0-alpine3.23'
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

		stage('test') {
			agent {
				docker {
					image 'node:24.13.0-alpine3.23'
					reuseNode true
				}
			}

			steps {
				sh '''
					test -f build/index.html
					npm run test
				'''
			}
		}
	}

	post {
		always {
			junit 'test-results/junit.xml'
		}
	}
}
