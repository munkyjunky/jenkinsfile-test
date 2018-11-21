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

        stage("Build") {

          agent {
            docker { image 'docker:stable' }
          }

          steps {
              unstash 'all'
              sh 'docker build .'
          }

        }

    }

}
