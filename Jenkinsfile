pipeline {

    agent any

    stages {

        stage("Build") {

            steps {
                checkout scm
                sh '''nvm install
                nvm use
                npm install
                npm test'''
            }

        }

    }

}
