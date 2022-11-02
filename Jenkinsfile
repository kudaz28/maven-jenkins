pipeline {
  agent any
  tools {
        maven "M3" 
   }
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
   stage ("Sonarqube Analysis - SASTs") 
      {
        steps 
        {
          withSonarQubeEnv('SonarQube') 
          {
             sh "mvn sonar:sonar -Dsonar.projectKey=maven-jenkins-pipeline -Dsonar.host.url=35.197.246.176"   
          }
          /*script 
          {
            def qualitygate = waitForQualityGate()
            if (qualitygate.status != "OK") {
               error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
            }
          }*/
          timeout(time: 1, unit: 'MINUTES') 
          {
             script 
             {
               waitForQualityGate abortPipeline: true
             }
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
