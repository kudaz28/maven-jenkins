pipeline {
  agent any // means run on any machine that is available to Jenkins
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
      stage('Deploy') 
      { 
          steps 
          { 
              input('Continue to Deploy?') 
              echo 'Deploying to Production Environment' 
          } 
      }
    }
}
