pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Installation de Python et pip') {
            steps {
                script {
                    echo 'Installation de Python et pip sans sudo'
                    sh '''
                        if ! command -v python3 &> /dev/null
                        then
                            echo "Python n'est pas installé, installation..."
                            curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
                            python3 get-pip.py
                        else
                            echo "Python déjà installé"
                        fi
                    '''
                    sh 'python3 --version'
                    sh 'pip3 --version'
                }
            }
        }

        stage('Installation de Pre-commit') {
            steps {
                script {
                    echo 'Installation de pre-commit'
                    sh '''
                        pip3 install pre-commit
                    '''
                }
            }
        }

        stage('Exécution de Pre-commit') {
            steps {
                script {
                    echo 'Exécution de pre-commit'
                    sh '''
                        pre-commit install
                        pre-commit run --all-files
                    '''
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Processus de build'
            }
        }

        stage('Post Actions') {
            steps {
                echo 'Pipeline terminé.'
            }
        }
    }

    post {
        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}
