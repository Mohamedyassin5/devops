pipeline {
    agent any

    tools {
        jdk 'JAVA17'          // Nom exact du JDK configuré dans Jenkins
        maven 'Maven-3.6.3'   // Nom exact de Maven configuré dans Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('t')                 // Ton token SonarQube
        SONAR_HOST  = 'http://192.168.56.73:9000'     // URL de ton SonarQube
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Mohamedyassin5/devops.git',
                    credentialsId: 'be8674b5-7e9e-433f-9ac1-ec156c203b7f' // Ton credential GitHub
            }
        }

        stage('Build') {
            steps {
                script {
                    sh "mvn clean package"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    sh """
                    mvn sonar:sonar \
                        -Dsonar.projectKey=timesheet-devops \
                        -Dsonar.host.url=${SONAR_HOST} \
                        -Dsonar.login=${SONAR_TOKEN}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Build and SonarQube analysis completed successfully.'
        }
        failure {
            echo 'Something went wrong... Check the logs.'
        }
    }
}
