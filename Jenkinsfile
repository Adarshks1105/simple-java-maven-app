Pipeline{
  agent any
  tools{
    maven 'Maven'
  }
  stages{
    stage('SonarQube scan'){
      steps{
        bat 'mvn sonar:sonar \
        -Dsonar.projectKey=simple-java-maven-app \
        -Dsonar.host.url=http://localhost:9000 \
        -Dsonar.login=89cdaebe990d43caa9ad7316f74f8056ae50b104'
      }
    } 
    stage('Build'){
      steps{
        bat 'mvn -B -DskipTests clean package'
      }
    }
    stage('Test'){
      steps{
        bat 'mvn test'
      }
    post{
      always{
        junit 'target/surefire-reports/*.xml'
       }
      }
    } 
    stage('Publish to Artifactory'){
      steps{
        rtMavenDeployer(
          id:'deployer',
          serverId:'Pipeline_repository@artifactory',
          releaseRepo:'Pipeline_repository',
          snapshotRepo:'Pipeline_repository'
          )
        rtMavenRun(
          pom:'pom.xml',
          goals:'clean install',
          deployerId:'deployer'
          )
        rtPublishBuildInfo(
          serverId:'Pipeline_repository@artifactory'
          )
      }
    }
  }
}
          
        
