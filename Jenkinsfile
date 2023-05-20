pipeline {
    
    agent any
     tools {
  maven 'MAVEN3'
  }
    environment {
       oldVersion = ''  
       mavenPom = ''                            
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
                
                println(env.oldVersion)
                env.mavenPom = readMavenPom file: 'MyWebApp/pom.xml'
                if (env.mavenPom.equals(env.oldVersion))
                {
                  println("Equal")
                }
                else {
                  println("Not Equal")
                }
                env.oldVersion = env.mavenPom
                println(env.oldVersion)
                nexusArtifactUploader artifacts: [[artifactId: 'MyWebApp', classifier: '', file: "MyWebApp/target/MyWebApp.jar", type: 'jar']], credentialsId: "NEXUS_CRED", groupId: 'com.dept.app', nexusUrl: '20.231.52.56:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'myapp', version: "${mavenPom.version}"
          
                }
                 }
        }
    
     
}
 post{
        always{
            emailext to: "amrt462@gmail.com",
            subject: "jenkins build:${currentBuild.currentResult}: ${env.JOB_NAME}",
            body: "${currentBuild.currentResult}: Job ${env.JOB_NAME}\nMore Info can be found here: ${env.BUILD_URL}"
        }
    }
}
