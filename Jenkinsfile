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

    stage ("Code scan") {
            steps {
             script{
               withSonarQubeEnv("sonarqube") {
                sh "mvn sonar:sonar -f MyWebApp/pom.xml"
               }
              
             }  
            }
        }

    
     stage("SonarQube Quality Gate check") {
      steps{
              waitForQualityGate abortPipeline: true
      }
     }
    

    stage ('Build') {
      steps {
      sh 'mvn clean install -f MyWebApp/pom.xml'
      }
    }

    
    stage('Excute Ansible') {
            steps {
             ansiblePlaybook credentialsId: 'private-key', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'playbook.yml'  }
        }

     stage ("Upload to Nexus") {
            steps {
                script{

                def mavenPom = readMavenPom file: 'MyWebApp/pom.xml'
                nexusArtifactUploader artifacts: [[artifactId: 'MyWebApp', classifier: '', file: "MyWebApp/target/MyWebApp.jar", type: 'jar']], credentialsId: "NEXUS_CRED", groupId: 'com.dept.app', nexusUrl: '40.121.81.242:8081/', nexusVersion: 'nexus3', protocol: 'http', repository: 'myapp', version: "${mavenPom.version}"

                }
                 }
        }


}
 post{
        always{
             emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        }
    }
}
