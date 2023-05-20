pipeline {
    
    agent any
     tools {
  maven 'MAVEN3'
  }
  
    environment {
        version = ''
       
    }
    stages {
        stage('Code checkout') {
            steps {
             checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/amrtarek456/myRepoForJavaApp.git']])
    }
        }
      

    stages {
        stage('Excute Ansible') {
            steps {
             ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'apache.yml'  }
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
                nexusArtifactUploader artifacts: [[artifactId: 'MyWebApp', classifier: '', file: "MyWebApp/target/MyWebApp.jar", type: 'jar']], credentialsId: "NEXUS_CRED", groupId: 'com.dept.app', nexusUrl: '20.231.52.56:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'myapp', version: "${mavenPom.version}"
          
                }
                 }
        }
    
     
}
 post{
        always{
            emailext to: "amrt462@gmail.com",
            subject: "Jenkins",
            body: "Jenkins",
            attachLog: true
        }
    }
}
