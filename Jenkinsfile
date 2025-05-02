pipeline {
    agent any

    environment {
        DOCKER_USERNAME = 'medaminegdouraucar'
        IMAGE_NAME = 'tp3-jenkins'
        TIME_TAG = "${new Date().format('yyyyMMdd-HHmmss')}"
        DOCKER_IMAGE = "${DOCKER_USERNAME}/${IMAGE_NAME}:${TIME_TAG}"
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git branch: 'main',url :'https://github.com/Medaminegdoura/jenkins-tp3'
            }
        }

        stage('Construire l\'image Docker') {
            steps {
                script {
                    sh "docker build -t $DOCKER_IMAGE ."
                }
            }
        }

        stage('Pousser l\'image Docker') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKER_TOKEN')]) {
                    sh "echo \$DOCKER_TOKEN | docker login -u $DOCKER_USERNAME --password-stdin"
                    sh "docker push $DOCKER_IMAGE"
                }
            }
        }

        stage('Déployer sur Kubernetes') {
            steps {
                script {
                    sh 'minikube kubectl -- apply -f deployment.yaml'
                    sh 'minikube kubectl -- apply -f service.yaml'
                }
            }
        }
    }
}
