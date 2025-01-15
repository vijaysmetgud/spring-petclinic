pipeline {
    agent any

    tools {
        // Ensure Maven is installed and available in Jenkins.
        maven 'Maven 3.6'  // Replace 'Maven 3.6' with your specific tool name configured in Jenkins
    }

    stages {
        stage('SCM') {
            steps {
                // Pull the code from the repository
                git branch: 'main', url: 'https://github.com/vijaysmetgud/spring-petclinic.git'
            }
        }

        stage('BUILD') {
            steps {
                // Build the project using Maven
                sh 'mvn clean package'
            }
        }

        stage('SonarQube analysis') {
            steps {
                // Run SonarQube analysis using the Maven command
                sh '''mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=spc-key \
                    -Dsonar.host.url=http://65.2.152.26:9000 \
                    -Dsonar.login=sqp_8eb3aa094b77804b212a143e9c9b9cd179685bd8'''
            }
        }
    }

    post {
        always {
            // Clean up workspace after build, optional but recommended.
            cleanWs()
        }
        success {
            echo 'Build and SonarQube analysis completed successfully.'
        }
        failure {
            echo 'Build or SonarQube analysis failed.'
        }
    }
}
