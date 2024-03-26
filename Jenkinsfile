pipeline {
     agent {
    kubernetes {
      yaml """
kind: Pod
metadata:
  name: node
spec:
  containers:
  - name: node
    image: node:14-alpine
    command:
    - cat
    tty: true
"""
    }
  }
     stages {
         stage('Build Step 1') {
             steps {
                 echo 'Building 456...'
                 sh 'pwd'
             }
             post {
                 always {
                     jiraSendBuildInfo site: 'cloudbees.atlassian.net'
                 }
             }
         }
         stage('Deploy - Staging') {
             /*when {
                 branch 'main'
             }*/
             steps {
                 echo 'Deploying to Staging from main...'
             }
             post {
                 always {
                     jiraSendDeploymentInfo environmentId: 'us-stg-1', environmentName: 'us-stg-1', environmentType: 'staging'
                 }
             }
         }
         stage('Deploy - Production') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying to Production from main...'
            }
            post {
                always {
                    jiraSendDeploymentInfo environmentId: 'us-prod-1', environmentName: 'us-prod-1', environmentType: 'production'
                }
            }
         }
     }
 }
