pipeline {
    agent {
        docker {
            image 'node:18-bullseye'
            args '-u root:root'
        }
    }

    environment {
        GIT_REPO_URL = 'https://github.com/molkaBenjeddouuu/projet-devsecops.git'
        NODE_VERSION = '18'
    }

    stages {
        // Étape 1: Vérification du code (Checkout)
        stage('Checkout') {
            steps {
                git url: "${env.GIT_REPO_URL}", branch: 'main'
            }
        }

        // Étape 2: Vérification du dépôt
        stage('Vérification du dépôt') {
            steps {
                script {
                    sh 'ls -al'
                }
            }
        }

        // Étape 3: Vérification du fichier package.json
        stage('Vérification du fichier package.json') {
            steps {
                script {
                    sh '''
                        echo "Vérification de l'existence du fichier package.json"
                        ls -al
                        cat package.json || echo "Le fichier package.json n'existe pas."
                    '''
                }
            }
        }

        // Étape 4: Installation des dépendances
        stage('Install Dependencies') {
            steps {
                script {
                    sh '''
                        echo "Vérification de Node.js et npm"
                        node -v
                        npm -v
                        echo "Installation des dépendances"
                        npm install
                        echo "Installation de Snyk"
                        npm install -g snyk  # Ajoutez cette ligne pour installer Snyk globalement
                    '''
                }
            }
        }

        // Étape 5: Analyse de sécurité avec Snyk
        stage('Snyk Security Scan') {
            steps {
                script {
                    sh 'snyk test'
                }
            }
        }

        // Étape 6: Scan de sécurité avec OWASP ZAP
        stage('OWASP ZAP Scan') {
            steps {
                script {
                    sh 'zap-cli quick-scan --self-contained --start-url http://ton-app-url'
                }
            }
        }

        // Étape 7: Push les modifications vers GitHub (si nécessaire)
        stage('Push to GitHub') {
            steps {
                script {
                    sh '''
                        git config --global user.email "ton-email@example.com"
                        git config --global user.name "TonNom"
                        git add .
                        git commit -m "Mise à jour depuis Jenkins Pipeline"
                        git push origin main
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminée.'
        }

        success {
            echo 'Pipeline réussie !'
        }

        failure {
            echo 'Pipeline échouée.'
        }
    }
}
