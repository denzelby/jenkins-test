pipeline {
    agent any
    stages {
        stage('Debug pipeline') {
            steps {
                sh 'printenv'
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
                input "Does the staging environment look ok?"
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