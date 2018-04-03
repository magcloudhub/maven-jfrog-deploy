node {
    def server = Artifactory.server('pavan.jfrog.io')
    def buildInfo = Artifactory.newBuildInfo()
    def rtMaven = Artifactory.newMavenBuild()
    
    stage ('Checkout & Build') {
        git url: 'https://github.com/pavants52/jfrog-project-examples.git'
    }
 
    stage ('Unit Test') {
        rtMaven.tool = 'apache-maven-3.5.2' // Tool name from Jenkins configuration
        rtMaven.run pom: 'maven-example/pom.xml', goals: 'clean test'
    }
    
    stage('SonarQube Analysis') {
        //rtMaven.run pom: 'maven-example/pom.xml', goals: 'clean org.jacoco:jacoco-maven-plugin:prepare-agent package sonar:sonar -Dsonar.host.url=https://sonarcloud.io -Dsonar.organization=pattabhi -Dsonar.login=df5bb81bae9ba310d6a38135b957227ba6ecd32c '
     
          
      }
    
    stage ('Artifactory configuration') {
        // Obtain an Artifactory server instance, defined in Jenkins --> Manage..:
         
        rtMaven.tool = 'apache-maven-3.5.2' // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
        rtMaven.deployer.deployArtifacts = false // Disable artifacts deployment during Maven run
     }
            
    stage ('Install') {
        rtMaven.run pom: 'maven-example/pom.xml', goals: 'install', buildInfo: buildInfo
     }
 
    stage ('Deploy') {
        rtMaven.deployer.deployArtifacts buildInfo
    }
        
    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}
