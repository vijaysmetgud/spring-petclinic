pipeline {
    agent any

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
                sh '''mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=spc-key \
                    -Dsonar.host.url=https://13.234.204.178:9000 \
                    -Dsonar.login=sqp_0d303fdc59469028d648406d5faa888a4f450616'''
            }
        }
    }
}
