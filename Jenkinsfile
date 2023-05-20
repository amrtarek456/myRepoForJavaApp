//this will grab user - who is running the job
def user
node {
  wrap([$class: 'BuildUser']) {
    user = env.BUILD_USER_ID
  }
  
  emailext mimeType: 'text/html',
                 subject: "[Jenkins]${currentBuild.fullDisplayName}",
                 to: "amretareke@outlook.com",
                 body: '''<a href="${BUILD_URL}input">click to approve</a>'''
}
pipeline {
    
    agent any
     tools {
  maven 'MAVEN3'
  }
    
  
    stages {
        stage('Code checkout') {
            steps {
             checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/amrtarek456/myRepoForJavaApp.git']])
    }
        }
      
    stage ('Build') {
      steps {
      sh 'mvn clean install -f MyWebApp/pom.xml'
      }
    }
    
    
    
     stage ("Upload to Nexus") {
            steps {
                script{
                def mavenPom = readMavenPom file: 'MyWebApp/pom.xml'
                if (mavenPom.equals(oldVersion))
                {
                  println("Equal")
                }
                else {
                  println("Not Equal")
                }
                def oldVersion = mavenPom
                nexusArtifactUploader artifacts: [[artifactId: 'MyWebApp', classifier: '', file: "MyWebApp/target/MyWebApp.jar", type: 'jar']], credentialsId: "NEXUS_CRED", groupId: 'com.dept.app', nexusUrl: '54.152.4.219:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'myapp', version: "${mavenPom.version}"
          
                }
                 }
        }
    
     
}

}
