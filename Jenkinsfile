pipeline {
    agent any

    environment {
        // L'ID des credentials Docker Hub que tu as créé dans Jenkins
        DOCKER_CRED = 'DOCKER_HUB_CRED'
        IMAGE_NAME = 'ikbelabidi/student-management'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Ikbel88-bit/mohamedikbel_aidi_4sim1.git'
            }
        }

        stage('Maven Build') {
            steps {
                // Génère le JAR nécessaire pour Docker
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKER_CRED}", 
                    usernameVariable: 'DOCKER_USERNAME', 
                    passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh """
                        echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin
                        docker push ${IMAGE_NAME}:${IMAGE_TAG}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline terminé avec succès ! L'image Docker est sur Docker Hub."
        }
        failure {
            echo "Le pipeline a échoué. Vérifie les logs pour plus de détails."
        }
    }
}
