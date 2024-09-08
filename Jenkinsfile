pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('s7valdes-dockerhub')
        SONARQUBE_ENV = 'Sonar'
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
        timestamps()
    }

    tools {
        maven 'maven'
    }

    stages {
        stage('Test') {
            steps {
                echo 'Running Maven tests...'
                sh 'mvn clean test'
                sh 'mvn  package'
                sh 'ls -la target' // Check if the target directory exists
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    echo 'Running SonarQube analysis...'
                    withSonarQubeEnv("${env.SONARQUBE_ENV}") { 
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

stage('Build Image') {
    steps {
        script {
            echo 'Building Docker image...'
            sh 'ls -la target' // List contents of the target directory to confirm JAR file is present
            sh '''
            cd ${WORKSPACE}
            docker build -t versage/s7valdes:${BUILD_NUMBER} .
            '''
        }
    }
}

stage('Push image') {
           when{ 
         expression {
           env.GIT_BRANCH == 'origin/main' }
           }
           steps {
               sh '''
               versage/s7valdes:${BUILD_NUMBER}
               '''
           }
        }

    }

    post {
        always {
            echo 'Cleaning up...'
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
