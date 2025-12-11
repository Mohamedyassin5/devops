pipeline {
    agent any
    
    tools {
        maven 'M3'  // Assurez-vous que Maven est configuré dans Jenkins
        jdk 'JDK8'  // Votre projet utilise Java 8 (java.version=1.8)
    }
    
    stages {
        stage('Hello') {
            steps {
                echo 'Pipeline Spring Boot avec SonarQube'
            }
        }
        
        stage('Checkout') {
            steps {
                echo 'Récupération du code depuis Git...'
                git branch: 'main',
                    url: 'https://github.com/Mohamedyassin5/devops.git'
            }
        }
        
        stage('Explorer structure') {
            steps {
                script {
                    // Vérifions la structure
                    sh '''
                        echo "=== Structure du dépôt ==="
                        ls -la
                        echo "=== Recherche du projet ==="
                        find . -name "pom.xml" -type f
                    '''
                }
            }
        }
        
        stage('Build') {
            steps {
                echo 'Build du projet Spring Boot...'
                script {
                    // Trouve automatiquement le dossier avec pom.xml
                    def pomFile = sh(script: 'find . -name "pom.xml" -type f | head -1', returnStdout: true).trim()
                    
                    if (pomFile) {
                        echo "Projet trouvé : ${pomFile}"
                        def projectDir = new File(pomFile).parent
                        
                        // Va dans le dossier du projet
                        dir(projectDir) {
                            sh 'pwd'
                            sh 'ls -la'
                            sh 'mvn clean compile'
                        }
                    } else {
                        error "ERREUR : Aucun projet Maven trouvé !"
                    }
                }
            }
        }
        
        stage('Tests') {
            steps {
                echo 'Exécution des tests...'
                script {
                    def pomFile = sh(script: 'find . -name "pom.xml" -type f | head -1', returnStdout: true).trim()
                    dir(new File(pomFile).parent) {
                        sh 'mvn test'
                    }
                }
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                echo 'Analyse de la qualité du code avec SonarQube...'
                script {
                    def pomFile = sh(script: 'find . -name "pom.xml" -type f | head -1', returnStdout: true).trim()
                    dir(new File(pomFile).parent) {
                        // Option 1 : Si SonarQube est configuré dans Jenkins
                        withSonarQubeEnv('SonarQube') {
                            sh 'mvn sonar:sonar'
                        }
                        
                        // Option 2 : En ligne de commande directe (si l'option 1 échoue)
                        // sh '''
                        //     mvn sonar:sonar \
                        //     -Dsonar.host.url=http://192.168.56.73:9000 \
                        //     -Dsonar.projectKey=timesheet-devops \
                        //     -Dsonar.projectName="Timesheet DevOps" \
                        //     -Dsonar.java.binaries=target/classes
                        // '''
                    }
                }
            }
        }
        
        stage('Package') {
            steps {
                echo 'Création du JAR...'
                script {
                    def pomFile = sh(script: 'find . -name "pom.xml" -type f | head -1', returnStdout: true).trim()
                    dir(new File(pomFile).parent) {
                        sh 'mvn package -DskipTests'
                        sh 'ls -la target/*.jar'
                    }
                }
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline réussi !'
            echo '📊 Rapport SonarQube disponible sur : http://192.168.56.73:9000'
            echo '📦 JAR créé dans le dossier target/'
        }
        failure {
            echo '❌ Pipeline échoué !'
        }
        always {
            echo '🧹 Nettoyage terminé.'
        }
    }
}
