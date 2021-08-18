pipeline {
    environment {
        registry = "sgilz/rampup-ui"
        registryCredential = 'dockerhub'
        dockerImage = ''
        BACK_HOST="internal-sgilz-api-elb-1069246019.us-west-1.elb.amazonaws.com"
    }
    agent any
    stages {
        stage('Cloning our Git') {
            steps {
                git 'git@github.com:santiago-gilz/movie-analyst-ui.git'
            }
        }

        stage('Building Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("$registry:$BUILD_NUMBER", "--build-arg BACK_HOST=$BACK_HOST")
                }
            }
        }

        stage('Deploying Docker Image to Dockerhub') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                    dockerImage.push()
                    }
                }
            }
        }

        stage('Cleaning Up') {
            steps{
                sh "docker rmi --force $registry:$BUILD_NUMBER"
            }
        }
    }
}
