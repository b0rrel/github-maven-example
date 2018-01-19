node {   
   def mvnHome
   def server = Artifactory.server 'art'
   
   properties([[$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '10']]]);
   
   stage('Preparation') { // for display purposes
      
      // Get some code from a GitHub repository
      git 'https://github.com/Bankers88/github-maven-example.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'M3'
   }
   stage('Build') {
      // Run the maven build
      dir('example') {
         sh "'${mvnHome}/bin/mvn' -Dmaven.test.failure.ignore clean package"
      }
   }
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archive 'target/*.jar'
   }
   stage('Deploy') {
      def uploadSpec = """{
         "files": [
             {
               "pattern": "github-maven-example-*-sources.jar",
               "target": "test-repo/test"
             }
          ]
       }"""
      server.upload(uploadSpec)
   }
}
