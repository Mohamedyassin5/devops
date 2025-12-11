pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('t') // Ton token SonarQube
    }

    stages {
        stage('Hello') {
            steps {
                echo 'Mon premier pipeline avec Jenkinsfile !'
            }
        }

        stage('Checkout') {
            steps {
                echo 'Code récupéré depuis Git'
                git branch: 'main',
                    url: 'https://github.com/Mohamedyassin5/devops.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Build en cours...'
                dir('timesheet-devops') {
                    sh 'mvn clean install -DskipTests'
                }
            }
        }

        stage('MVN SONARQUBE') {
            steps {
                echo 'Analyse SonarQube en cours...'
                dir('timesheet-devops') {
                    withSonarQubeEnv('sonarqube') { // nom du serveur configuré dans Jenkins
                        sh "mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN} -DskipTests"
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline réussi !'
        }
        failure {
            echo '❌ Pipeline échoué !'
        }
    }
}
