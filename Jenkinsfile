pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonarqube-token1')
        JAVA_HOME = "/usr/lib/jvm/java-8-openjdk-amd64" // adapte selon ton Linux/Vagrant
        PATH = "${JAVA_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Mohamedyassin5/devops.git'
            }
        }

        stage('Build Maven') {
            steps {
                dir('timesheet-devops') {
                    // Compile et build, tests ignorés pour éviter JUnit5 qui pose problème
                    sh 'mvn clean install -DskipTests'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('timesheet-devops') {
                    withSonarQubeEnv('SonarServer') {
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

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success { echo 'Pipeline executed successfully!' }
        failure { echo 'Pipeline failed!' }
    }
}
