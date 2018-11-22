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

              script {
                GIT_TAG = sh(returnStdout: true, script: 'git tag --points-at HEAD').trim()
              }

              echo "${GIT_TAG}"

              stash name: 'all', includes: '**'
          }
        }

        stage("Check tag") {
          agent { docker { image 'alpine' } }

          steps {
            echo "${GIT_TAG}"
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

        stage("Building version tag") {
          when {
            beforeAgent true
            expression { GIT_TAG ==~ /v\\d+.\\d+.\\d+/ }
          }
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
