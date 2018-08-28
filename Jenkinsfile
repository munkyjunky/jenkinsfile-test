pipeline {

    agent any

    stages {

        stage("Build") {

            steps {
                checkout scm
                sh whoami
                sh nvm install
            }

        }

    }

}
