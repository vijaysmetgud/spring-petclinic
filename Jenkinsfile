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
            def scannerHome = tool name: 'sonar_scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation';
                withSonarQubeEnv('SonarQube') { 
                sh "${scannerHome}/bin/sonar-scanner"
            }        
        }
    }
}     
