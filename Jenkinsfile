pipeline {
  agent any
  tools {
        maven "M3" 
   }
mvn clean verify sonar:sonar \
  -Dsonar.projectKey=maven-pipeline \
  -Dsonar.host.url=http://34.142.46.33:9000 \
  -Dsonar.login=sqp_ac86161d4429026f6293501f9e78dca1dd1ebe3f
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
              sh "mvn sonar:sonar -Dsonar.projectKey=maven-pipeline -Dsonar.host.url=http://34.142.46.33:9000"  
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
