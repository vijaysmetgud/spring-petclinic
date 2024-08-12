pipeline {
    agent any

    stages {
        stage('Checkout code') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/vijaysmetgud/spring-petclinic.git']])
            }
        }
     
     stage('Compile Sample Application') {
            steps {
                dir('/var/lib/jenkins/workspace/spring-petclinic'){
                sh '/var/lib/jenkins/workspace/test/apache-maven-3.9.4/bin/mvn compile'
            }
           }
        }
    stage('Test Sample Application') {
            steps {
                dir('/var/lib/jenkins/workspace/spring-petclinic'){
                sh '/var/lib/jenkins/workspace/spring-petclinic/src/test/apache-maven-3.9.4/bin/mvn compile'
            }
           }
        }
     stage('Package Sample Application') {
            steps {
                dir('/var/lib/jenkins/workspace/spring-petclinic'){
                sh '/var/lib/jenkins/workspace/spring-petclinic/src/test/apache-maven-3.9.4/bin/mvn package'
            }
           }
        }
     stage('Upload Jfrog Artifact') {
            steps {
                dir('/var/lib/jenkins/workspace/spc day build/src/test/'){
                rtServer (
    id: 'mainserver',
    url: 'http://34.201.63.174:8081//artifactory',
        // If you're using username and password:
    //username: 'admin',
    //password: 'password',
        // If you're using Credentials ID:
        credentialsId: 'jfrog-jenkins-connection',
        // If Jenkins is configured to use an http proxy, you can bypass the proxy when using this Artifactory server:
        bypassProxy: true,
        // Configure the connection timeout (in seconds).
        // The default value (if not configured) is 300 seconds: 
        timeout: 300
)    
                rtUpload (
    serverId: 'mainserver',
    spec: '''{
          "files": [
            {
              "pattern": "spring-petclinic.jar",
              "target": "libs-snapshot-local"
            }
         ]
    }''',
    )    
    }
     } 
    }
stage('Download Jfrog Artifact') {
            steps {
                dir('/'){
rtDownload (
    serverId: 'mainserver',
    spec: '''{
          "files": [
            {
              "pattern": "libs-snapshot-local/",
              "target": ""
            }
          ]
    }''',
)
}
}
}

stage('Tomcat Prerequisites Installation') {
            steps {
                sh 'sudo apt update'
                sh 'sudo apt install openjdk-17-jre -y'
           }
        }

     stage('Install Apache Tomcat Server') {
            steps {
                sh 'wget https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz'
                sh 'tar -xzvf apache-tomcat-8.5.24.tar.gz'
                sh 'sudo chown -R cloudadmin:cloudadmin apache-tomcat-8.5.24'
                sh 'sudo cp tomcat-users.xml apache-tomcat-8.5.24/conf/tomcat-users.xml'
                sh 'sudo cp context.xml apache-tomcat-8.5.24/webapps/manager/META-INF/context.xml'
                sh 'sudo sed -i "s/8080/8082/g" apache-tomcat-8.5.24/conf/server.xml'
        }
}        
     stage('Deploy Sample Application To Tomcat Server') {
            steps {
                sh 'sudo cp addressbook/addressbook_main/target/addressbook.war apache-tomcat-8.5.24/webapps/'
                sh 'sudo runuser -l cloudadmin -c "/tmp/workspace/test/apache-tomcat-8.5.24/bin/startup.sh"'
            }
        }
        
    }
}
