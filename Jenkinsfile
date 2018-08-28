pipeline {

    agent any

    stages("Build") {

        steps {
            checkout scm
            sh '''#!/bin/bash -l
            nvm install
            nvm use
            npm install
            npm test'''
        }

    }

}
