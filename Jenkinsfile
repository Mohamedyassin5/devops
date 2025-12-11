pipeline {
    agent any
    
    tools {
        maven 'M2_HOME'
    }
    
    stages {
        stage('Explorer la structure') {
            steps {
                script {
                    echo "=== DÉBUT DE L'EXPLORATION ==="
                    
                    sh '''
                        echo "📍 Répertoire courant: $(pwd)"
                        echo ""
                        echo "📁 CONTENU:"
                        ls -la
                        echo ""
                        echo "🔍 RECHERCHE POM.XML:"
                        find . -name "pom.xml" -type f
                        echo ""
                        echo "📊 TOUS LES FICHIERS:"
                        find . -type f | head -20
                    '''
                }
            }
        }
    }
    
    post {
        always {
            echo "✅ Exploration terminée"
        }
    }
}
