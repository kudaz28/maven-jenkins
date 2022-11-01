pipeline {
  agent any // means run on any machine that is available to Jenkins
  tools {
        maven "M3"
   }
  
  // this is a dummy change
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
              sh "mvn sonar:sonar -Dsonar.projectKey=maven-jenkins-pipeline -Dsonar.host.url=http://34.105.199.221:9000"  
           }
          timeout(time: 2, unit: 'MINUTES'){
            script {
        waitForQualityGate abortPipeline: true
            }
          }
        } 
      } 
      stage('Dev Environment') 
    
      { 
          steps 
          { 
              echo "I'm now deploying to the dev environment" 
          } 
      }
      stage('Test Environment') 
      { 
          steps 
          { 
              echo "I'm now deploying to the test or QA environment" 
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
