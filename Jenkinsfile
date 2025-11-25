pipeline {
    agent any

    environment {
        DOCKER_CRED = 'DOCKER_HUB_CRED' // ID des credentials Jenkins
    }

    stages {
        stage('GIT') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Ikbel88-bit/mohamedikbel_aidi_4sim1.git'
            }
        }

        stage('Compile Stage') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ikbelabidi/student-management:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CRED}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh '''
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        docker push ikbelabidi/student-management:latest
                    '''
                }
            }
        }
    }
}
