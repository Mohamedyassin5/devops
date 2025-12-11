pipeline {
    agent any

    // Outils configurés dans Jenkins → Global Tool Configuration
    tools {
        jdk 'JAVA_HOME'       // Nom exact du JDK configuré
        maven 'M2_HOME'       // Nom exact de Maven configuré ou "Maven" si installé automatiquement
    }

    environment {
        // Token SonarQube configuré dans Jenkins → Credentials
        SONAR_TOKEN = credentials('t')  
        SONAR_HOST  = 'http://192.168.56.73:9000'   // URL de ton SonarQube
    }

    stages {

        stage('Checkout') {
            steps {
                // Récupère le code depuis Git
                git branch: 'main', url: 'https://github.com/Mohamedyassin5/devops.git'
            }
        }

        stage('Build') {
            steps {
                // Compile et package sans tests
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Analyse SonarQube
                sh """
                    mvn sonar:sonar \
                        -Dsonar.projectKey=timesheet-devops \
                        -Dsonar.host.url=${SONAR_HOST} \
                        -Dsonar.login=${SONAR_TOKEN}
                """
            }
        }

        stage('Tests') {
            steps {
                // Optionnel : exécuter les tests unitaires
                sh 'mvn test'
            }
        }
    }

    post {
        success {
            echo 'Build, SonarQube scan, and tests completed successfully!'
        }
        failure {
            echo 'Something went wrong... Check the logs.'
        }
    }
}
