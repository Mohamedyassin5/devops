pipeline {
    agent any
    
    stages {
        stage('Vérifier la structure') {
            steps {
                script {
                    echo "=== DÉBUT DU PIPELINE ==="
                    
                    sh '''
                        echo "📍 Répertoire courant:"
                        pwd
                        
                        echo ""
                        echo "📁 Contenu de la racine:"
                        ls -la
                        
                        echo ""
                        echo "🔍 Recherche de pom.xml:"
                        find . -name "pom.xml" -type f
                        
                        echo ""
                        echo "🌳 Structure des dossiers:"
                        find . -maxdepth 3 -type d | sort
                    '''
                }
            }
        }
        
        stage('Analyse SonarQube') {
            when {
                expression { 
                    // Exécute seulement si on trouve un projet
                    def pomFiles = findFiles(glob: '**/pom.xml')
                    return pomFiles.size() > 0
                }
            }
            steps {
                script {
                    echo "🔧 Construction et analyse..."
                    
                    // Cherche le premier projet Maven
                    def pomFiles = findFiles(glob: '**/pom.xml')
                    def projectDir = new File(pomFiles[0].path).parent
                    
                    echo "Projet trouvé dans: ${projectDir}"
                    
                    dir(projectDir) {
                        sh '''
                            echo "=== Build Maven ==="
                            mvn clean compile test
                            
                            echo "=== Analyse SonarQube ==="
                            mvn sonar:sonar -Dsonar.host.url=http://192.168.56.73:9000
                        '''
                    }
                }
            }
        }
    }
    
    post {
        always {
            echo "✅ Pipeline terminé"
        }
    }
}
