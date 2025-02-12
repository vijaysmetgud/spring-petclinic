pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKER_IMAGE = "vsmetgud/springpetgitlab"  // Replace with your desired Docker image name
        SONARQUBE_URL = "http://your-sonarqube-server"  // Replace with your SonarQube server URL
        SONARQUBE_TOKEN = credentials('sonarqube-token')  // Assuming you've stored the SonarQube token as a Jenkins secret
        DOCKER_REGISTRY = "docker.io"  // Docker registry URL (e.g., Docker Hub, AWS ECR, etc.)
        DOCKER_REGISTRY_CREDENTIALS = credentials('docker-registry-credentials')  // Credentials for Docker registry
    }

    stages {
        // Clone the repository (optional if you don't already have the code)
        stage('Clone') {
            steps {
                script {
                    echo "Cloning the repository"
                    // Clone the repository (assuming it's configured in your Jenkins job or manually specify the URL)
                    git 'https://github.com/vijaysmetgud/spring-petclinic.git'  // Replace with your repo URL
                }
            }
        }

        // Build stage
        stage('Build') {
            steps {
                script {
                    echo "Building the application"
                    // Clean and build the Spring Boot project with Maven
                    sh 'mvn clean install -DskipTests'
                }
            }
        }

        // Test stage
        stage('Test') {
            steps {
                script {
                    echo "Running tests"
                    // Run tests using Maven
                    sh 'mvn test'
                }
            }
        }

        // SonarQube analysis stage
        stage('SonarQube Analysis') {
            steps {
                script {
                    echo "Running SonarQube analysis"
                    // Perform SonarQube analysis
                    withSonarQubeEnv('SonarQube') {  // Ensure 'SonarQube' is configured in Jenkins
                        sh 'mvn sonar:sonar -Dsonar.projectKey=spring-petclinic -Dsonar.host.url=${SONARQUBE_URL} -Dsonar.login=${SONARQUBE_TOKEN}'
                    }
                }
            }
        }

        // Dockerize the application using Kaniko
        stage('Dockerize with Kaniko') {
            steps {
                script {
                    echo "Dockerizing the application using Kaniko"
                    // Run Kaniko to build the Docker image
                    sh '''
                    /kaniko/executor --context $WORKSPACE --dockerfile $WORKSPACE/Dockerfile --destination $DOCKER_REGISTRY/$DOCKER_IMAGE:latest
                    '''
                }
            }
        }

        // Trivy scanning stage
        stage('Trivy Scan') {
            steps {
                script {
                    echo "Running Trivy scan for vulnerabilities"
                    // Run Trivy to scan the Docker image for vulnerabilities
                    sh 'trivy image --exit-code 1 --severity HIGH,CRITICAL $DOCKER_REGISTRY/$DOCKER_IMAGE:latest'
                }
            }
        }
    }

    post {
        always {
            cleanWs()  // Clean workspace after the build
        }

        success {
            echo "Pipeline completed successfully"
        }

        failure {
            echo "Pipeline failed"
        }
    }
}
