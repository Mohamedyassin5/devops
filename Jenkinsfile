pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('t') // Ton token SonarQube stocké dans Jenkins Credentials
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Mohamedyassin5/devops.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('MVN SONARQUBE') {
            steps {
                sh """
                mvn sonar:sonar \
                    -Dsonar.projectKey=timesheet-devops \
                    -Dsonar.host.url=http://192.168.56.73:9000 \
                    -Dsonar.login=${SONAR_TOKEN}
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline terminé avec succès !'
        }
        failure {
            echo 'Erreur dans le pipeline !'
        }
    }
}
