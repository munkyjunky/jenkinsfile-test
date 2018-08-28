pipeline {

    agent none

    stages {

        stage("Build") {

            agent {
                docker { image 'node:8' }
            }

            steps {
                checkout scm
                sh 'npm install'
                sh 'npm test'
            }

        }

    }

}
