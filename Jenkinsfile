pipeline {
    agent any

    environment {
        // SONARQUBE_SERVER = 'SonarQube'  // The name of the SonarQube server configured in Jenkins
        ARTIFACTORY_SERVER = 'Artifactory'  // The name of the Artifactory server configured in Jenkins
        ARTIFACTORY_REPO = 'libs-release-local'  // Artifactory repository to upload artifacts
        ARTIFACTORY_PASSWORD = 'myjfrog@123'
        ARTIFACTORY_USER = 'myjfrog@123'
        
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

        // stage('SonarQube analysis') {
        //     steps {
        //         script {
        //             // Perform the SonarQube analysis securely using the SonarQube token
        //             withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
        //                 withSonarQubeEnv(SONARQUBE_SERVER) {
        //                     sh '''mvn clean verify sonar:sonar \
        //                         -Dsonar.projectKey=spc-key \
        //                         -Dsonar.host.url=https://13.234.204.178:9000 \
        //                         -Dsonar.login=${SONAR_TOKEN}'''
        //                 }
        //             }
        //         }
        //     }
        // }

        stage('Upload to Artifactory') {
            steps {
                script {
                    // Define Artifactory server and upload specification
                    withCredentials([usernamePassword(credentialsId: 'artifactory-creds', passwordVariable: 'ARTIFACTORY_PASSWORD', usernameVariable: 'ARTIFACTORY_USER')]) {
                        def server = Artifactory.server(ARTIFACTORY_SERVER)
                        def uploadSpec = """{
                            "files": [
                                {
                                    "pattern": "target/*.jar",  // The artifact files to upload
                                    "target": "${ARTIFACTORY_REPO}/com/example/spring-petclinic/"
                                }
                            ]
                        }"""

                        // Upload the artifacts to Artifactory
                        server.upload(uploadSpec)
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
