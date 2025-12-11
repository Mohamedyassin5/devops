pipeline {
    agent any
    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }
    tools {
        jdk 'JAVA17'
        maven 'Maven'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                sh "mvn sonar:sonar -Dsonar.login=${SONAR_TOKEN}"
            }
        }
    }
}
