pipeline {
    agent any
    
    environment {
        SONAR_TOKEN = credentials('t')  // Ton token SonarQube
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
                git url: 'https://github.com/Mohamedyassin5/devops.git', branch: 'main'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Build en cours...'
                dir('timesheet-devops') {      // <-- dossier contenant le pom.xml
                    sh 'mvn clean install'
                }
            }
        }
        
        stage('MVN SONARQUBE') {
            steps {
                echo 'Analyse SonarQube en cours...'
                dir('timesheet-devops') {      // <-- même dossier pour Sonar
                    withSonarQubeEnv('My SonarQube Server') { // Nom du serveur Sonar dans Jenkins
                        sh "mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN"
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
