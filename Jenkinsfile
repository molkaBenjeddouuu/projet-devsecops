pipeline {
    agent any
    environment {
        GIT_REPO_URL = 'https://github.com/molkaBenjeddouuu/projet-devsecops.git'
    }
    stages {
        stage('Installation de Python et pip') {
            steps {
                script {
                    echo "Installation de Python et pip"
                    sh '''
                        # Vérifier si Python est installé
                        if ! command -v python3 &> /dev/null
                        then
                            echo "Python n'est pas installé, installation..."
                            sudo apt update
                            sudo apt install python3 python3-pip -y
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
                        # Installer pre-commit si nécessaire
                        if ! command -v pre-commit &> /dev/null
                        then
                            echo "pre-commit n'est pas installé, installation..."
                            pip3 install pre-commit
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
