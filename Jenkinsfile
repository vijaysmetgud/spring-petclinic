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
git branch: 'main', url:
'https://github.com/vijaysmetgud/spring-petclinic.git'
}
}
stage ('Artifactory configuration') {
steps {
rtMavenDeployer (
id: "spc-maven",
serverId: "jrog-jenkins-connection",
releaseRepo: 'libs-release-local/',
snapshotRepo: 'libs-snapshot-local/'
)
}
}
stage ('Exec Maven') {
steps {
rtMavenRun (
tool: 'Maven 3.9.6', // Tool name from Jenkins configuration
pom: ('pom.xml'),
goals: 'clean install',
deployerId: "admin"
// buildName: "${JOB_NAME}",
// buildNumber: "${BUILD_ID}"
)
}
}
stage ('Publish build info') {
steps {
rtPublishBuildInfo (
serverId: "jrog-jenkins-connection"
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
