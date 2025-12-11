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
                git branch: 'main',
                    url: 'https://github.com/Mohamedyassin5/devops.git',
                    credentialsId: 'Mohamedyassin5'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Build en cours...'
                sh 'mvn --version'
                sh 'mvn clean compile'
            }
        }
        
        stage('Tests') {
            steps {
                echo 'Exécution des tests...'
                sh 'mvn test'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                echo 'Analyse de la qualité du code avec SonarQube...'
                
                // Méthode 1 : Avec avecSonarQubeEnv (si configuré dans Jenkins)
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar'
                }
                
                // OU Méthode 2 : En ligne de commande directe
                // sh '''
                //     mvn sonar:sonar \
                //     -Dsonar.host.url=http://192.168.56.73:9000 \
                //     -Dsonar.projectKey=devops-project \
                //     -Dsonar.projectName="DevOps Project" \
                //     -Dsonar.login=votre_token_ici
                // '''
            }
        }
        
        stage('Package') {
            steps {
                echo 'Création du package JAR...'
                sh 'mvn package -DskipTests'
                sh 'ls -la target/*.jar'
            }
        }
    }
    
    post {
        success {
            echo '✅ Pipeline réussi !'
            echo '📊 Consultez le rapport SonarQube sur : http://192.168.56.73:9000'
        }
        failure {
            echo '❌ Pipeline échoué !'
        }
        always {
            // Nettoyage
            echo '🧹 Nettoyage terminé.'
        }
    }
}