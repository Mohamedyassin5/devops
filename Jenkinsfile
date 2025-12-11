pipeline {
    agent any

    environment {
        // Token SonarQube enregistré dans Jenkins
        SONAR_TOKEN = credentials('sonarqube-token1')

        // Définir explicitement JAVA_HOME et Maven
        JAVA_HOME = "/usr/lib/jvm/java-17-openjdk-amd64/"
        PATH = "${env.JAVA_HOME}/bin:/opt/apache-maven-3.6.3/bin:${env.PATH}"
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
                    sh "mvn clean install -DskipTests"
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube analysis..."
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

        stage("Quality Gate") {
            steps {
                echo "Waiting for SonarQube Quality Gate result..."
                timeout(time: 2, unit: 'MINUTES') {
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
            echo "Pipeline failed!"
        }
    }
}
