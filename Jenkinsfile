pipeline {

    agent none

    stages {

        stage("Build") {

            agent {
                docker { image 'node:8' }
                environment { HOME="." }
            }

            steps {
                checkout scm
                sh 'npm install'
                sh 'npm test'
            }

        }

    }

}
