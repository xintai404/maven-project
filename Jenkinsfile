pipeline{
  agent any
  stages{
    stage('Build'){
      steps{
        withmaven(){
          sh 'mvn clean package'
        }
      }
      post{
        success{
          echo "Now archiving..."
          archiveArtifacts artifacts: '**/target/*.war'
        }
      }
    }
    stage('Deploy to Staging'){
      steps{
        build job: deploy-to-stage
      }
    
    }
    stage('Deploy to Product'){
      steps{
        timeout(time:5, unit:'DAYS'){
          input message: 'Approval Production deployment?'
        }
        
        build job: 'deploy-to-prod'
      }
      post{
        success{
          echo 'deployed to production'
        }
        failure{
          echo 'deployment fail'
        }
      }
    }
  
  }


}
