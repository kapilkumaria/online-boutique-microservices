pipeline {
    agent {
        label 'gcp-microservices' // Ensure the job runs on the 'gcp-microservices' Jenkins slave
    }

    environment {
        REPO_URL = 'https://github.com/kapilkumaria/microservices-demo-sample.git' // Replace with your repository URL
        SCAN_TOOL = 'SonarQube' // Specify the scanning tool, e.g., SonarQube, Trivy, etc.
        SONARQUBE_URL = 'http://localhost:9000' // Replace with your SonarQube URL
        SONARQUBE_CREDENTIALS = credentials('sonarqube-token') // Use Jenkins credentials for secure authentication
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    echo "Cloning the repository from ${REPO_URL}"
                }
                git url: "${REPO_URL}", branch: 'main' // Clone the main branch; specify if different
            }
        }

    }
    

    post {
        success {
            echo 'Code pull and scan completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check logs for details.'
        }
    }
}

