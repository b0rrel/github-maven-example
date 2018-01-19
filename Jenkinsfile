node {   
   def mvnHome
   def server
   
   properties([[$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '10']]]);
   
   stage('Clone') { // for display purposes
      // Clone code from GitHub repository
      git 'https://github.com/Bankers88/github-maven-example.git'
   }
   
   stage('Artifactory configuration') {
      server = Artifactory.server 'art'
      rtMaven = Artifactory.newMavenBuild()
      rtMaven.tool = 'M3'
      rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
      rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
      rtMaven.deployer.deployArtifacts = false // Disable artifacts deployment during Maven run
      
      buildInfo = Artifactory.newBuildInfo()
   }
   
   stage ('Test') {
      rtMaven.run pom: 'example/pom.xml', goals: 'clean test'
   }
   
   stage ('Install') {
        rtMaven.run pom: 'example/pom.xml', goals: 'install', buildInfo: buildInfo
    }
 
    stage ('Deploy') {
        rtMaven.deployer.deployArtifacts buildInfo
    }
        
    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}
