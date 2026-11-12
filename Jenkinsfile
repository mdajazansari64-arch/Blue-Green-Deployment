
pipeline {
    agent any

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
                echo 'GitHub code checkout successful'
            }
        }

        stage('Docker Check') {
            steps {
                bat 'docker --version'
            }
        }

        stage('Build Blue Image') {
            steps {
                bat 'docker build -t blue-app:v1 -f app/Dockerfile app/blue'
            }
        }

        stage('Build Green Image') {
            steps {
                bat 'docker build -t green-app:v2 -f app/Dockerfile app/green'
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-pat-new',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    bat '''
                        @echo off
                        echo %DOCKER_PASS%| docker login -u "%DOCKER_USER%" --password-stdin
                    '''

                    bat 'docker tag blue-app:v1 %DOCKER_USER%/blue-app:v1'
                    bat 'docker tag green-app:v2 %DOCKER_USER%/green-app:v2'

                    bat 'docker push %DOCKER_USER%/blue-app:v1'
                    bat 'docker push %DOCKER_USER%/green-app:v2'

                    bat 'docker logout'
                }
            }
        }
    }
}
