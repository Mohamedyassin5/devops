pipeline {
    agent any

    tools {
        maven 'Maven 3.9.0' // Remplace par le nom de ton Maven dans Jenkins
        jdk 'Java 17'       // Remplace par la version de JDK installée sur Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('sonarqube-token1') // Ton token SonarQube dans Jenkins
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning repository..."
                git branch: 'main', url: 'https://github.com/Mohamedyassin5/devops.git'
            }
        }

        stage('Build Maven') {
            steps {
                echo "Running Maven build..."
                dir('timesheet-devops') {
                    sh "mvn clean install" // Exécute avec les tests
                }
            }
        }

        stage('Run Tests') {
            steps {
                echo "Running tests separately..."
                dir('timesheet-devops') {
                    sh "mvn test"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube analysis..."
                script {
                    withSonarQubeEnv('SonarServer') { // Nom exact du serveur SonarQube configuré dans Jenkins
                        dir('timesheet-devops') {
                            sh """
                                mvn sonar:sonar \
                                  -Dsonar.projectKey=devops \
                                  -Dsonar.host.url=http://192.168.56.73:9000 \
                                  -Dsonar.login=${SONAR_TOKEN}
                            """
                        }
                    }
                }
            }
        }

        stage("Quality Gate") {
            steps {
                echo "Waiting for SonarQube Quality Gate result..."
                timeout(time: 5, unit: 'MINUTES') { // Timeout sécurisé pour projets moyens/gros
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed! Check logs above for details."
        }
    }
}
