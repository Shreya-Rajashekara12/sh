pipeline {
    agent any

    environment {
        IMAGE = "shreyalr/sh:v1"
        // It is safer to hardcode the ID in the step, but we will keep this for reference
        DOCKER_CREDS_ID = "dockerhub-creds"
    }
}

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Shreya-Rajashekara12/sh.git'
            }
        }
        
        stage('Build') {
            steps {
                bat 'docker build -t devp .'
            }
        }

        stage('Tag') {
            steps {
                bat "docker tag devp %IMAGE%"
            }
        }

        stage('Login') {
            steps {
                // We use withCredentials to securely bind your Jenkins secret to variables
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds', 
                    usernameVariable: 'USER', 
                    passwordVariable: 'PASS'
                )]) {
                    // Windows uses %VAR% instead of $VAR
                    bat "docker login -u %USER% -p %PASS%"
                }
            }
        }

        stage('Push') {
            steps {
                bat "docker push %IMAGE%"
            }
        }
    }
    
    post {
        always {
            // Good practice: Logout after pushing to keep the agent clean
            bat "docker logout"
        }
    }
}
