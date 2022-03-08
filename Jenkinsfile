pipeline {
  agent any
  stages {
    stage('Stopping current container') {
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh '''docker stop ${GROUP}.staging.${PROJECT}
'''
              echo 'Stopped current staging container'
            }

          }
        }

    stage('Deleting current staging container') {
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh 'docker rm ${GROUP}.staging.${PROJECT}'
          echo 'Removed current staging container'
        }

      }
    }

    stage('Copying .env') {
      steps {
        sh 'cp .env.example .env'
      }
    }

    stage('Building and deploying') {
      steps {
        sh 'docker-compose --file ./docker/staging/docker-compose.yml --env-file ./.env up -d --build'
        echo 'Completed successfully!'
      }
    }

  }
  environment {
    GROUP = 'menke'
    PROJECT = 'testproject'
  }
}
