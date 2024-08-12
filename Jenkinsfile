pipeline{
    agent any

    stages{
        stage('SCM'){
            steps{
                git branch: 'main', url:'https://github.com/vijaysmetgud/spring-petclinic.git'
                
            }
        }
        stage('BUILD') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('SonarQube analysis') {
            def scannerHome = tool 'SonarQubeScanner 6.0.0.4432';
                withSonarQubeEnv('SonarQube') { 
                sh "${scannerHome}/bin/sonar-scanner"
            }        
        }
    }
}     
