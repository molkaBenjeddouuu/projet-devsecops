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
        stage('Checkout') {
            steps {
                git url: "${env.GIT_REPO_URL}", branch: 'main'
            }
        }

        stage('Vérification des outils') {
            steps {
                script {
                    echo "Vérification de l'environnement"
                    sh '''
                        echo "Affichage du PATH"
                        echo $PATH
                        echo "Vérification des commandes"
                        which zap-cli || echo "zap-cli non trouvé"
                        which npm || echo "npm non trouvé"
                        which node || echo "node non trouvé"
                    '''
                }
            }
        }

        // Ajoute les autres étapes ici...
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
