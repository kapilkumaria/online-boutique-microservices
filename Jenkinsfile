pipeline {
    agent {
        label 'gcp-microservices' // Ensure the job runs on the 'gcp-microservices' Jenkins slave
    }

    environment {
        REPO_URL = 'https://github.com/kapilkumaria/microservices-demo-sample.git' // Replace with your repository URL
        SONARQUBE_SERVER = 'SonarQube' // Name you configured in Jenkins for SonarQube
        SONARQUBE_URL = 'http://100.25.44.199:9000'
        SONARQUBE_TOKEN = credentials('sonar-token') // Jenkins credential for SonarQube token
        SRC_DIR = 'src' // Root directory for microservices
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

        stage('Code Quality Analysis') {
            steps {
                script {
                    def services = ['adservice', 'cartservice', 'checkoutservice', 'currencyservice', 'emailservice', 
                                    'frontend', 'loadgenerator', 'paymentservice', 'productcatalogservice', 
                                    'recommendationservice', 'shippingservice', 'shoppingassistantservice']

                    for (service in services) {
                        dir("${SRC_DIR}/${service}") {
                            withSonarQubeEnv(SONARQUBE_SERVER) {
                                sh """
                                    mvn clean verify sonar:sonar \
                                    -Dsonar.projectKey=${service} \
                                    -Dsonar.projectName=${service} \
                                    -Dsonar.host.url=${SONARQUBE_URL} \
                                    -Dsonar.login=${SONARQUBE_TOKEN}
                                """
                            }
                        }
                    }
                }
            }
        }
    } 
    

    post {
        success {
            echo 'Code pull, code quality analysis completed successfully for all microservices!'
        }
        failure {
            echo 'Pipeline failed. Please check logs for details.'
        }
    }
}

