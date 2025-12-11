pipeline {
    agent any
    tools {
        jdk 'JAVA17'        // Le JDK installé dans Jenkins (configuré dans Global Tool Configuration)
        maven 'Maven'       // Maven installé dans Jenkins
    }
    environment {
        SONAR_TOKEN = credentials('sonar-token-id') // Le token que tu as créé dans SonarQube
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ton-utilisateur/ton-projet.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                sh "mvn sonar:sonar -Dsonar.projectKey=ton_project_key -Dsonar.host.url=http://192.168.56.73:9000 -Dsonar.login=$SONAR_TOKEN"
            }
        }
    }
}
