# Runing a Simple container from git repo - CI/CD Pipeline

This repository contains an automated CI/CD pipeline for deploying a web-based Container using **Jenkins**, **Podman**, and **Git**.

## 🚀 The Pipeline Script (Jenkinsfile)

Copy this code into a file named `Jenkinsfile` in the root of your repository:

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Pulls the latest code from this repo
                git branch: 'main', url: '[https://github.com/Manilehra09/repo-fot-jenkin.git](https://github.com/Manilehra09/repo-fot-jenkin.git)'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Make the shell script executable and run it
                    sh "chmod +x deploy.sh"
                    sh "./deploy.sh"
                }
            }
        }
    }
    
    post {
        success {
            echo "------------------------------------------------------------"
            echo "SUCCESS: Container has been deployed to port 8003."
            echo "------------------------------------------------------------"
        }
        failure {
            echo "------------------------------------------------------------"
            echo "FAILURE: The deployment failed!"
            echo "Checking container status..."
            // Shows if the container is stuck or didn't start
            sh "sudo podman ps -a | grep lms-webserver || echo 'Container not found.'"
            echo "Please check the Console Output above for specific errors."
            echo "------------------------------------------------------------"
        }
    }
}
