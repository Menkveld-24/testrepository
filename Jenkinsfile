pipeline {
  agent any
  stages {
    stage('Stopping current container') {
      agent any
      steps {
        sh '''docker stop ${GROUP}.staging.${PROJECTNAAM}
'''
        echo 'Stopped current staging container'
      }
    }

  }
  environment {
    GROUP = 'iea'
    PROJECT = 'testlaravel'
  }
}