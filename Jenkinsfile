pipeline {
  agent any
  triggers {
      pollSCM('0 * * ? * *')
  }
  stages {
    stage('Debug pipeline') {
      parallel {
        stage('Debug pipeline') {
          steps {
            sh 'printenv'
          }
        }
        stage('debug2') {
          steps {
            echo 'some message'
            build(job: 'Sport server/feature-ci', wait: true)
          }
        }
      }
    }
    stage('Build') {
      steps {
        sh './gradlew clean build -x test'
      }
    }
    stage('Test') {
      steps {
        sh './gradlew test'
      }
      post {
        always {
          junit '**/build/test-results/test/*.xml'

        }
      }
    }
    stage('Deploy') {
      when {
        branch 'master'
      }
      steps {
        input 'Does the staging environment look ok?'
        sh 'echo deploying!'
      }
    }
    stage('Integration tests') {
      when {
        branch 'master'
      }
      steps {
        sh 'echo testing!'
      }
    }
  }
}