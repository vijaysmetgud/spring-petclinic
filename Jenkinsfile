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
            steps {
          //  def scannerHome = tool 'SonarQubeScanner 6.0.0.4432';
             //   withSonarQubeEnv('SonarQube') { 
            //    sh "${scannerHome}/bin/sonar-scanner"
                sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=sonartest -Dsonar.host.url=http://18.119.138.43:9000 -Dsonar.login=388baf314f0da1a9c27d4b024384664bdcf157b7'
            }        
        }
    }
}     
