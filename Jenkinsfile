def availableClusters = ['default', 'site', 'ems-private', 'ems-public']

pipeline {
    agent any
    parameters {
        choice name: 'clusterName', description: 'test param', choices: availableClusters
    }
    environment {
        CLUSTER_NAME = "${(clusterName != 'default') ? '-PclusterName='+clusterName : ''}"
    }
    stages {
        stage('Debug pipeline') {
            steps {
                sh 'printenv'
            }
        }
        stage('Build') {
            steps {
                sh './gradlew clean build -x test $CLUSTER_NAME'
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