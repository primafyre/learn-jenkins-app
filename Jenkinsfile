pipeline {
	agent any

	stages {
		stage('with docker') {
			agent {
				docker {
					image 'node:24.13.0-alpine3.23'
					reuseNode true
				}
			}

			steps {
				sh '''
                    echo "With Docker"
                    node --version
                    ls -ahl
                    touch container-yes.txt
                '''
			}
		}
		stage('w/o docker') {
			steps {
				sh '''
                    echo "Without Docker"
                    ls -ahl
                    touch container-no.txt
                '''
			}
		}
	}
}
