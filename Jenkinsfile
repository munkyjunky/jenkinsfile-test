pipeline {

    agent none

    options {
      skipDefaultCheckout true
    }

    stages {

        stage("Install Dependencies") {

            agent { docker { image 'node:10' } }

            environment { HOME="." }

            steps {
                checkout scm
                sh 'npm ci'
                stash name: 'all', includes: '**'
            }

        }

        stage('Verify') {

          parallel {

              stage("Code Style") {

                  agent { docker { image 'node:10' } }

                  steps {
                      unstash 'all'
                      sh 'npm run eslint'
                  }

              }

              stage("Test") {

                  agent { docker { image 'node:10' } }

                  steps {
                      unstash 'all'
                      sh 'npm test'
                  }

              }

          }

        }

        stage("Building version tag") {
          when { buildingTag() }
          agent { docker { image 'alpine' } }

          steps {
            sh 'echo "Version tag!"'
            sh "echo $TAG_NAME"
          }
        }

        stage("Build") {

          agent {
            docker {
              image 'docker:stable'
              args '-v /var/run/docker.sock:/var/run/docker.sock --entrypoint=\'\' -u root'
            }
          }

          steps {
              unstash 'all'
              sh 'docker build .'
          }

        }

    }

}
