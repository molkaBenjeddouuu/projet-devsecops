pipeline {
    agent any
    environment {
        GIT_REPO_URL = 'https://github.com/molkaBenjeddouuu/projet-devsecops.git'
    }
    stages {
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
