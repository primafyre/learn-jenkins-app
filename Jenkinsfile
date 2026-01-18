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

                    npm install --global bun

                    bun install --production --no-save --frozen-lockfile
                    bun run build

                	ls -ahl
                '''
			}
		}

		stage('test') {
			steps {
				echo 'test stage'
			}
		}
	}
}
