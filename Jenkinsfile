stage('MVN SONARQUBE') {
    steps {
        script {
            // Exécute l'analyse SonarQube avec Maven
            sh """
            mvn sonar:sonar \
                -Dsonar.projectKey=timesheet-devops \
                -Dsonar.host.url=http://192.168.56.73:9000 \
                -Dsonar.login=${SONAR_TOKEN}
            """
        }
    }
}
