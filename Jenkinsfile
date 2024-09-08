pipeline {
    agent any

    environment {
    DOCKERHUB_CREDENTIALS = credentials('s7valdes-dockerhub')
    }
    options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
    disableConcurrentBuilds()
    timeout(time: 10, unit: 'MINUTES')
    timestamps()
    }

    tools {
        maven 'maven' // Assumes Maven is installed and configured in Jenkins with this name
    }

    environment {
        SONARQUBE_ENV = 'Sonar' // Corrected the block name
    }

    stages {
        stage('Test') {
            steps {
                echo 'Running Maven tests...'
                sh 'mvn clean test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    echo 'Running SonarQube analysis...'
                    withSonarQubeEnv("${env.SONARQUBE_ENV}") { // Use the environment variable
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }

        stage('Login') {
           steps {
               sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('build-image') {
            steps {
                sh '''
                cd ${WORKSPACE}
               
                docker build -t versage/s7valdes:${BUILD-NUMBER} .
                '''
            }
        
    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs() // Cleans up the workspace
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
