pipeline {
    agent any
    environment {
        GIT_REPO_URL = 'https://github.com/molkaBenjeddouuu/projet-devsecops.git'
    }
    stages {
        stage('Installation de Python et pip') {
            steps {
                script {
                    echo "Installation de Python et pip sans sudo"
                    sh '''
                        # Vérifier si Python est installé
                        if ! command -v python3 &> /dev/null
                        then
                            echo "Python n'est pas installé, installation..."
                            # Installer Python dans le répertoire utilisateur sans sudo
                            curl -sSL https://install.python-poetry.org | python3 -
                        else
                            echo "Python déjà installé"
                        fi
                    '''
                }
            }
        }
        stage('Installation de Pre-commit') {
            steps {
                script {
                    echo "Installation de Pre-commit"
                    sh '''
                        # Installer pre-commit via pip si Poetry échoue
                        python3 -m pip install --user pre-commit
                    '''
                }
            }
        }
        stage('Exécution de Pre-commit') {
            steps {
                script {
                    echo "Exécution des hooks pre-commit"
                    sh '''
                        pre-commit run --all-files
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
