pipeline {

   agent none

   stages {

     // Run unit tests
     stage('test') {
       agent any
       steps {
         checkout scm
         sh 'ls -l'
       }
     }

     // Build distributables
     stage('build') {

       when {
           expression {
             node {
               GIT_TAG = sh(returnStdout: true, script: "git tag --sort version:refname | tail -1").trim()
               println GIT_TAG
               return GIT_TAG ==~ /(?i)(release)/
             }
          }
       }

       parallel {

         stage('parallel-1') {
            agent any
            steps {
                checkout scm
                sh 'echo "Running parallel 1"'
            }
         }

         stage('parallel-2') {
            agent any
            steps {
              checkout scm
              sh 'echo "Running parallel 2"'
            }
         }

       }

     }

   }
}
