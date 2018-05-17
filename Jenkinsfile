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
    post {
        failure {
            slackSend channel: 'ci-server',
                    color: 'bad',
                    message: "Pipeline (<${env.BUILD_URL}|${currentBuild.fullDisplayName}>) failed! @everyone"
        }
        success {
            slackSend channel: 'ci-server',
                    color: 'good',
                    message: "Pipeline (<${env.BUILD_URL}|${currentBuild.fullDisplayName}>) completed successfully."
        }
        always {
            slackSend channel: 'ci-server', message: "debug:  cb=${currentBuild.dump()}\n env=${env.dump()}"
        }
    }
}