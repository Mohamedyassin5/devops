pipeline {
    agent any
    
    stages {
        stage('Checkout et Analyse') {
            steps {
                script {
                    git url: 'https://github.com/Mohamedyassin5/devops.git'
                    
                    // Trouve le projet
                    sh 'find . -name "pom.xml"'
                    
                    // Exécute dans le dossier "timesheet-devops" (REMPLACEZ par votre vrai dossier)
                    dir('timesheet-devops') {
                        sh '''
                            mvn clean compile test
                            mvn sonar:sonar -Dsonar.host.url=http://192.168.56.73:9000
                        '''
                    }
                }
            }
        }
    }
}
