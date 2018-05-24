def availableClusters = ['default', 'site', 'ems-private', 'ems-public'].join('\n')

pipeline {
    agent any
    parameters {
        choice name: 'clusterName', description: 'test param', choices: availableClusters
    }
    environment {
        CLUSTER_NAME_PARAM = "${(params.clusterName != 'default') ? '-PclusterName=' + params.clusterName : ''}"
        BUILD_NUMBER_PARAM = "-Pbuild_number=b${env.BUILD_NUMBER}"

        GRADLE_PARAMS = "${env.CLUSTER_NAME_PARAM} ${env.BUILD_NUMBER_PARAM}"
    }
    stages {
        stage('Debug pipeline') {
            steps {
                sh 'printenv'
            }
        }
        stage('Build') {
            steps {
                sh './gradlew clean build -x test $GRADLE_PARAMS'
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