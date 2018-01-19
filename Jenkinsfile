node {   
   def mvnHome
   stage('Preparation') { // for display purposes
      // Set logRotator to 5 days
      options {
         buildDiscarder(logRotator(numToKeepStr: '5'))
      }
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
}
