pipeline {
    agent none  // Utilise agent none pour éviter de définir un agent global, et définis des agents spécifiques dans les étapes.

    environment {
        GIT_REPO_URL = 'https://github.com/molkaBenjeddouuu/projet-devsecops.git'
        NODE_VERSION = '18'
        SONARQUBE_URL = 'http://sonarqube:9000'  // URL de ton instance SonarQube
        SONARQUBE_TOKEN = credentials('SonarToken')  // Token d'accès SonarQube (configuré dans Jenkins)
    }

    stages {
        stage('Checkout') {
            agent any  // Utilise n'importe quel agent pour cette étape
            steps {
                git url: "${env.GIT_REPO_URL}", branch: 'main'
            }
        }

        stage('Vérification des outils') {
            agent any
            steps {
                script {
                    echo "Vérification de l'environnement"
                    sh '''
                        echo "Affichage du PATH"
                        echo $PATH
                        echo "Vérification des commandes"
                        which pre-commit || echo "pre-commit non trouvé"
                        which npm || echo "npm non trouvé"
                        which node || echo "node non trouvé"
                    '''
                }
            }
        }

        stage('Exécution de Pre-commit') {
            agent any
            steps {
                script {
                    echo "Exécution des hooks pre-commit"
                    sh '''
                        pre-commit run --all-files
                    '''
                }
            }
        }

        stage('Analyse SonarQube') {
            agent any
            steps {
                script {
                    echo "Analyse du code avec SonarQube"
                    sh '''
                        sonar-scanner \
                            -Dsonar.projectKey=projet-devsecops \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=${SONARQUBE_URL} \
                            -Dsonar.login=${SONARQUBE_TOKEN}
                    '''
                }
            }
        }

        stage('Tests unitaires avec JUnit') {
            agent any
            steps {
                script {
                    echo "Exécution des tests unitaires avec JUnit"
                    sh '''
                        npm install
                        npm test -- --reporter junit --output ./test-results
                    '''
                }
            }
        }

        stage('Création de l\'image Docker') {
            agent {
                docker {
                    image 'node:18-bullseye'
                    args '-u root:root'
                }
            }
            steps {
                script {
                    echo "Création de l'image Docker"
                    sh '''
                        docker build -t projet-devsecops .
                    '''
                }
            }
        }

        stage('Déploiement de Grafana et Prometheus') {
            agent any
            steps {
                script {
                    echo "Déploiement de Grafana et Prometheus pour la surveillance"
                    sh '''
                        docker-compose -f docker-compose-monitoring.yml up -d
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
