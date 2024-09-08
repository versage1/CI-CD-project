pipeline {
    agent any

    tools {
        maven 'maven' // Assumes Maven is installed and configured in Jenkins with this name
    }

    environmentm {
        SONARQUBE_ENV = 'Sonar' // Name of the SonarQube installation in Jenkins
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
                    withSonarQubeEnv('SonarQube') {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
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
