pipeline {

    agent none

    options {
      skipDefaultCheckout true
    }

    stages {

        stage("Checkout") {

          agent { docker { image 'golang' } }

          steps {
              checkout scm
              sh 'git fetch --tags'
              script {
                env.GIT_TAG = sh(returnStdout: true, script: 'git tag --points-at HEAD').trim()
              }
              stash name: 'all', includes: '**'
          }
        }

        stage("Install Dependencies") {

            agent { docker { image 'node:10' } }

            environment { HOME="." }

            steps {
                unstash 'all'
                sh 'npm ci'
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
          when {
            beforeAgent true
            expression { env.GIT_TAG ==~ /v\d+.\d+.\d+/ }
          }
          agent { docker { image 'alpine' } }

          steps {
            sh 'echo "Version tag!"'
          }
        }

        stage("Not version tag") {
          when {
            beforeAgent true
            not { expression { env.GIT_TAG ==~ /v\d+.\d+.\d+/ } }
          }
          agent { docker { image 'alpine' } }

          steps {
            sh 'echo "No version tag!"'
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
