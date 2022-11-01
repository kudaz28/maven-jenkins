pipeline {
  agent any
  tools {
        maven "M3" 
   }
  
//a new change
  mvn clean verify sonar:sonar \
  -Dsonar.projectKey=maven-jenkins-pipeline \
  -Dsonar.host.url=http://34.142.46.33:9000 \
  -Dsonar.login=sqp_94ab152d7d5a1ce827d7c73d910103940e6a5f20
  stages {
      stage('Build Artifact') 
      {
            steps 
            {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }  
       }
      stage('Test Maven - JUnit') 
      {
            steps 
            {
              //sh "mvn test"
              echo "Running unit tests"
            }
      }
    stage('Sonarqube Analysis - SAST')  
      { 
        steps  
        { 
           withSonarQubeEnv('SonarQube')  
           { 
              sh "mvn sonar:sonar -Dsonar.projectKey=maven-jenkins-pipeline -Dsonar.host.url=http://34.142.46.33:9000"  
           } 
        } 
      } 
      stage('Dev Environment') 
      { 
          steps 
          { 
              echo 'Deploying to Dev Environment' 
          } 
      }
    stage('Test Environment') 
      { 
          steps 
          { 
              echo 'Deploying to QA Environment' 
          } 
      }
    stage('UAT Environment') 
      { 
          steps 
          { 
              input('Continue to Deploy?') 
              echo 'Deploying to Production Environment' 
          } 
      }
    stage('Production Environment') 
      { 
          steps 
          { 
              input('Continue to Deploy?') 
              echo 'Deploying to Production Environment' 
          } 
      }
    }
}
