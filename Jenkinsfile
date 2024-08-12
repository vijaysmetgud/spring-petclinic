pipeline{
    agent any

    stages{
        stage('SCM'){
            steps{
                git branch: 'main', url:'https://github.com/dummyreposito/spring-petclinic.git'
                
            }
        }
        stage('BUILD') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('SonarQube analysis') {
            steps {
                script {
                // requires SonarQube Scanner 2.8+
                    scannerHome = tool 'SonarQube Scanner 2.17.2'
                }
                    withSonarQubeEnv('SonarQube Scanner') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}     
