pipeline {
    agent any

    tools {
        jdk 'Java-17'          // Nom exact du JDK dans Jenkins
        maven 'Maven-3.6.3'    // Nom exact de Maven dans Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN') // Ton token SonarQube
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Mohamedyassin5/devops.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // Nom du serveur SonarQube configuré dans Jenkins
                    sh "mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}"
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline terminé avec succès ✅'
        }
        failure {
            echo 'Pipeline échoué ❌'
        }
    }
}
