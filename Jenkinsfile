pipeline {
    agent any
    
    stages {
        stage('Hello') {
            steps {
                echo 'Mon premier pipeline avec Jenkinsfile !'
            }
        }
        
        stage('Checkout') {
            steps {
                echo 'Code récupéré depuis Git'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Build en cours...'
                sh 'mvn --version'
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
