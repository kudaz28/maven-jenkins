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
    stage ("Sonarqube Analysis - SASTs") 
      {
        steps 
        {
          withSonarQubeEnv('SonarQube') 
          {
             sh "mvn sonar:sonar -Dsonar.projectKey=maven-jenkins-pipeline -Dsonar.host.url=http://http://35.246.77.196:9000"   
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
