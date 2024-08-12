pipeline{
  agent any
    options{
    timeout(time:30, unit: 'MINUTES')
    }
  triggers{
    pollSCM('* * * * *')
  }
  tools{
    jdk 'jdk-17'
  }
stages{
    stage('git checkout'){
      steps{
        git branch: 'main', url: 'https://github.com/dummyreposito/spring-petclinic.git'
      }
    }
    stage ('Artifactory configuration') {
    steps {
    rtMavenDeployer (
    id: "spc_DEPLOYER",
    serverId: "mainserver",
    releaseRepo: 'libs-release-local',
    snapshotRepo: 'libs-snapshot-local'
    )
    }
    }
    stage ('Exec Maven') {
      steps {
        rtMavenRun (
            tool: 'Maven 3.9.6', // Tool name from Jenkins configuration
            pom: 'pom.xml',
            goals: 'clean install',
            deployerId: "spc_DEPLOYER"
            // buildName: "${JOB_NAME}",
            // buildNumber: "${BUILD_ID}"
            )
       }
    }
    stage ('Publish build info') {
        steps {
            rtPublishBuildInfo (
              serverId: "mainserver"
            )
        }
     }
    stage('Junit Results'){
      steps{
        junit testResults: '**/target/surefire-reports/TEST-*.xml'
      }
    }
 }
}
