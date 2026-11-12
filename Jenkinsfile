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
    }
}