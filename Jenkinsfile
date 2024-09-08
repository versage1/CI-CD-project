pipeline {
    agent any

    tools {
        maven 'Maven' // Assumes Maven is installed and configured in Jenkins with this name
    }



    stages {
        stage('Test') {
            steps {
                echo 'Running Maven tests...'
                sh 'mvn clean test'
            }
        }

        stage('SonarQube analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli:5.0.1'
                }
            }
            environment {
                CI = 'true'
                scannerHome = '/opt/sonar-scanner'
            }
            steps {
                withSonarQubeEnv('Sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
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
