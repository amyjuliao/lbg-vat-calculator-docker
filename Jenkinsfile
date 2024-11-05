pipeline {
    environment {
        registry = "amyjoconnor/vatcal"
        registryCredentials = "dockerhub_id"
        dockerImage = ""
    }
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/amyjuliao/lbg-vat-calculator-docker.git'
            }
        }
        stage('Update Dependencies') {
            steps {
                script {
                    // Update schema-utils
                    sh 'npm update schema-utils'
                    // Update all dependencies
                    sh 'npm update'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    // Rebuild the project
                    sh 'npm run build'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build(registry)
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', registryCredentials) {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage('Clean up') {
            steps {
                script {
                    sh 'docker image prune --all --force --filter "until=48h"'
                }
            }
        }
    }
}

