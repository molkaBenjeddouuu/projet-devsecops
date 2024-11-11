pipeline {
    agent any
    environment {
        GIT_REPO_URL = 'https://github.com/molkaBenjeddouuu/projet-devsecops.git'
    }
    stages {
        stage('Installation de Pre-commit') {
            steps {
                script {
                    echo "Installation de Pre-commit"
                    sh '''
                        # Installer pre-commit si nécessaire
                        if ! command -v pre-commit &> /dev/null
                        then
                            echo "pre-commit n'est pas installé, installation..."
                            pip install pre-commit
                        else
                            echo "pre-commit déjà installé"
                        fi
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
