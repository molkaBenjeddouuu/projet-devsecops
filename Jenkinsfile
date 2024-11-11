pipeline {
    agent any

    environment {
        // Définir des variables d'environnement si nécessaire
        GIT_REPO_URL = 'https://github.com/molkaBenjeddouuu/projet-devsecops.git'
    }

    stages {
        // Étape 1: Vérification du code
        stage('Checkout') {
            steps {
                git url: "${env.GIT_REPO_URL}", branch: 'main'
            }
        }

        // Étape 2: Installation des dépendances
        stage('Install Dependencies') {
            steps {
                script {
                    // Exemple : installation des dépendances si nécessaire
                    sh 'npm install' // Si ton projet utilise Node.js
                    // sh 'pip install -r requirements.txt' // Si ton projet utilise Python
                }
            }
        }

        // Étape 3: Analyse de sécurité avec Snyk
        stage('Snyk Security Scan') {
            steps {
                script {
                    // Exécuter Snyk pour scanner les vulnérabilités dans les dépendances
                    sh 'snyk test'
                }
            }
        }

        // Étape 4: Scan de sécurité avec OWASP ZAP
        stage('OWASP ZAP Scan') {
            steps {
                script {
                    // Exemple de scan rapide avec ZAP
                    sh 'zap-cli quick-scan --self-contained --start-url http://ton-app-url'
                }
            }
        }

        // Étape 5: Push les modifications vers GitHub (si nécessaire)
        stage('Push to GitHub') {
            steps {
                script {
                    // Si des modifications doivent être poussées vers GitHub
                    sh 'git push origin main'
                }
            }
        }
    }

    post {
        always {
            // Actions à faire après chaque exécution de pipeline
            echo 'Pipeline terminée.'
        }

        success {
            // Actions si la pipeline réussit
            echo 'Pipeline réussie !'
        }

        failure {
            // Actions si la pipeline échoue
            echo 'Pipeline échouée.'
        }
    }
}
