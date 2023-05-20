pipeline {
    
    agent any
     tools {
  maven 'MAVEN3'
  }
  
    environment {
        version = 0
       
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
                def oldVersion
                println(oldVersion)
                mavenPom = readMavenPom file: 'MyWebApp/pom.xml'
                if (mavenPom.equals(oldVersion))
                {
                  println("Equal")
                }
                else {
                  println("Not Equal")
                }
                oldVersion = mavenPom.version
                env.version = oldVersion

                println(oldVersion)
                println(env.version)
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
