pipeline {
    agent any
    environment {
        GIT_REPO_URL = 'https://github.com/molkaBenjeddouuu/projet-devsecops.git'
    }
    stages {
        stage('Vérification des outils') {
            steps {
                script {
                    echo "Vérification de l'environnement"
                    sh '''
                        echo "Affichage du PATH"
                        echo $PATH
                        echo "Vérification des commandes"
                        which git || { echo "Git non trouvé"; exit 1; }
                        which pre-commit || echo "pre-commit non trouvé"
                        which npm || echo "npm non trouvé"
                        which node || echo "node non trouvé"
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
