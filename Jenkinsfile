pipeline {
    agent any
    
    stages {
        stage('Vérifier structure') {
            steps {
                script {
                    git url: 'https://github.com/Mohamedyassin5/devops.git'
                    
                    sh '''
                        echo "=== VOICI CE QU'IL Y A DANS VOTRE REPOSITORY ==="
                        ls -la
                        echo ""
                        echo "=== CHERCHE POM.XML ==="
                        find . -name "pom.xml"
                        echo ""
                        echo "=== TOUS LES FICHIERS ==="
                        find . -type f | head -30
                    '''
                }
            }
        }
    }
}
