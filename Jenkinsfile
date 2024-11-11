pipeline {
    agent any

    environment {
        GIT_REPO_URL = 'https://github.com/molkaBenjeddouuu/projet-devsecops.git'  // URL du dépôt Git
        NODE_VERSION = '16'  // Définir la version de Node.js à utiliser
    }

    stages {
        // Étape 1: Vérification du code (Checkout)
        stage('Checkout') {
            steps {
                git url: "${env.GIT_REPO_URL}", branch: 'main'
            }
        }

        // Étape 2: Installation des dépendances
        stage('Install Dependencies') {
            steps {
                script {
                    // Installer nvm (Node Version Manager) si ce n'est pas déjà fait
                    sh '''
                        if ! command -v nvm &> /dev/null
                        then
                            echo "nvm non trouvé, installation..."
                            curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
                            export NVM_DIR="$HOME/.nvm"
                            [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
                        fi
                    '''
                    // Installer la version de Node.js via nvm
                    sh '''
                        export NVM_DIR="$HOME/.nvm"
                        [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
                        nvm install ${NODE_VERSION}
                        nvm use ${NODE_VERSION}
                    '''
                    // Installer les dépendances npm
                    sh 'npm install'
                }
            }
        }

        // Étape 3: Analyse de sécurité avec Snyk
        stage('Snyk Security Scan') {
            steps {
                script {
                    // Exécuter Snyk pour scanne
