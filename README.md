# Mastering Jenkins Pipelines: A Comprehensive Guide

Welcome to the Jenkins Pipeline learning repository! This guide covers everything from basic concepts to writing your first `Jenkinsfile`.

---

## 📖 What is a Jenkins Pipeline?
A **Pipeline** is a suite of plugins that supports implementing and integrating *continuous delivery pipelines* into Jenkins. In simple terms, it is a script that tells Jenkins the entire "path" your code takes from your machine to the production server.

> **Pipeline as Code:** Instead of clicking buttons in the Jenkins UI, we write the logic in a file called a `Jenkinsfile` and store it in Version Control (Git).

---

##  Core Components
| Component | Description |
| :--- | :--- |
| **Pipeline** | The user-defined model of a CD pipeline. |
| **Node** | A machine which is part of the Jenkins environment and is capable of executing a Pipeline. |
| **Stage** | A distinct subset of tasks (e.g., "Build", "Test", "Deploy"). |
| **Step** | A single task that tells Jenkins what to do at a particular point in time. |

---

##  Pipeline Syntax Types
There are two ways to write a Jenkins Pipeline:

### 1. Declarative Pipeline (Recommended)
This is the modern approach. It is easier to read and has a pre-defined structure.
* **Starts with:** `pipeline { ... }`
* **Best for:** Most users and standard CI/CD flows.

### 2. Scripted Pipeline
The traditional approach based on Groovy.
* **Starts with:** `node { ... }`
* **Best for:** Complex logic that requires custom programming.

---

##  Example: The "Hello World" Jenkinsfile
Save this code in a file named `Jenkinsfile` at the root of your project:

```groovy
pipeline {
    // Defines where the pipeline will run
    agent any 

    stages {
        stage('Fetch Code') {
            steps {
                echo 'Checking out code from Git...'
            }
        }
        
        stage('Build') {
            steps {
                echo 'Compiling project...'
                // Example: sh './mvnw clean install'
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running Unit Tests...'
                // Example: sh './mvnw test'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to Staging Environment...'
            }
        }
    }
    
    // Post-actions run after the stages
    post {
        always {
            echo 'Cleaning up workspace...'
        }
        success {
            echo 'Pipeline finished successfully!'
        }
        failure {
            echo 'Something went wrong. Sending alert...'
        }
    }
}
