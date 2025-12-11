pipeline {
    agent any
    
    environment {
        SONAR_TOKEN = credentials('t') // Ton token SonarQube enregistré dans Jenkins
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
                git branch: 'main', url: 'https://github.com/Mohamedyassin5/devops.git'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Build en cours...'
                sh 'mvn clean install'
            }
        }

        stage('MVN SONARQUBE') {
            steps {
                echo 'Analyse SonarQube en cours...'
                sh "mvn sonar:sonar -Dsonar.projectKey=timesheet -Dsonar.host.url=http://192.168.56.73:9000 -Dsonar.login=$SONAR_TOKEN"
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
