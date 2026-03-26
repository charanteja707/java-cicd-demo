pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'charantejadevops/java-cicd-demo'
    }

    stages {
        stage('Clone Code') {
            steps {
                git branch: 'main', url: 'https://github.com/charanteja707/java-cicd-demo.git'
            }
        }

        stage('Build JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'docker login -u $USERNAME -p $PASSWORD'
                    sh 'docker push $DOCKER_IMAGE:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker stop java-app || true'
                sh 'docker rm java-app || true'
                sh 'docker run -d -p 8080:8080 --name java-app $DOCKER_IMAGE:latest'
            }
        }
    }
}
