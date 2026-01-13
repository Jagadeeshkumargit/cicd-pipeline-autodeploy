pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "jagadeeshkumarkonne/train-schedule-app"
    }

    stages {

        stage('Build Application') {
            steps {
                echo 'Skipping Gradle build (legacy node dependency blocked)'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image'
                bat 'docker build -t %DOCKER_IMAGE%:latest .'
            }
        }

        stage('Push Docker Image') {
            steps {
                echo 'Pushing Image to DockerHub'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    bat '''
                    docker login -u %USER% -p %PASS%
                    docker push %DOCKER_IMAGE%:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                echo 'Deploying to Minikube Kubernetes'
                bat 'kubectl apply -f train-schedule-kube.yml'
            }
        }
    }
}
