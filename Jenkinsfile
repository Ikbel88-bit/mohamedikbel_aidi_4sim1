pipeline {
    agent any

    environment {
        DOCKER_CRED = 'DOCKER_HUB_CRED'
        SPRING_IMAGE_NAME = 'ikbelabidi/student-management'
        SPRING_IMAGE_TAG = 'latest'
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
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${SPRING_IMAGE_NAME}:${SPRING_IMAGE_TAG} ."
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
                        docker push ${SPRING_IMAGE_NAME}:${SPRING_IMAGE_TAG}
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
