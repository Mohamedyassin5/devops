pipeline {
    agent any
    
    stages {
        stage('Explorer la structure') {
            steps {
                script {
                    // Checkout
                    git url: 'https://github.com/Mohamedyassin5/devops.git'
                    
                    // Exploration complète
                    sh '''
                        echo "========================================="
                        echo "STRUCTURE COMPLÈTE DU REPOSITORY"
                        echo "========================================="
                        
                        echo ""
                        echo "1. RACINE:"
                        ls -la
                        
                        echo ""
                        echo "2. SOUS-DOSSIERS (niveau 1):"
                        for d in */; do
                            echo "=== $d ==="
                            ls -la "$d" | head -5
                        done
                        
                        echo ""
                        echo "3. RECHERCHE DE POM.XML:"
                        find . -name "pom.xml" -type f 2>/dev/null || echo "Aucun pom.xml trouvé"
                        
                        echo ""
                        echo "4. RECHERCHE DE PROJETS:"
                        find . -name "*.java" -o -name "*.py" -o -name "*.js" -o -name "Dockerfile" | head -20
                        
                        echo ""
                        echo "5. FICHIERS DE CONFIGURATION:"
                        find . -name "*.yml" -o -name "*.yaml" -o -name "*.properties" -o -name "application.*" | head -10
                        
                        echo ""
                        echo "6. ARBRE COMPLET:"
                        if command -v tree &> /dev/null; then
                            tree -L 3
                        else
                            echo "Commande 'tree' non disponible"
                            find . -maxdepth 3 -type d | sort
                        fi
                    '''
                }
            }
        }
    }
    
    post {
        always {
            echo 'Exploration terminée'
        }
    }
}
