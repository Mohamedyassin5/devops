pipeline {
    agent any
    
    stages {
        stage('Checkout et Analyse') {
            steps {
                script {
                    // Checkout du code
                    git url: 'https://github.com/Mohamedyassin5/devops.git'
                    
                    // Vérification de la structure
                    sh '''
                        echo "=== Structure complète du repository ==="
                        find . -type f -name "*.xml" -o -name "*.java" -o -name "Jenkinsfile" | sort
                        
                        echo "=== Recherche de projets Maven ==="
                        find . -name "pom.xml" -type f
                        
                        echo "=== Contenu du répertoire ==="
                        ls -la
                    '''
                    
                    // Essayer de trouver le bon dossier
                    dir('timesheet-devops') {
                        sh '''
                            echo "=== Dans timesheet-devops ==="
                            ls -la
                            find . -name "pom.xml" || echo "Pas de pom.xml ici"
                        '''
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline terminé'
        }
    }
}
