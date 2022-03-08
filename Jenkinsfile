pipeline {
  agent any
  stages {
    stage('Stopping current container') {
      parallel {
        stage('Stopping current container') {
          agent any
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh '''docker stop ${GROUP}.staging.${PROJECT}
'''
              echo 'Stopped current staging container'
            }

          }
        }

        stage('Stopping current containers database') {
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh '''docker stop ${GROUP}.staging.${PROJECT}.mysql
'''
              echo 'Stopped mysql container'
            }

          }
        }

      }
    }

    stage('Deleting current staging containers') {
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh 'docker rm ${GROUP}.staging.${PROJECT}'
          sh 'docker rm ${GROUP}.staging.${PROJECT}.mysql'
          echo 'Deleted old containers'
        }

      }
    }

    stage('Removing staging storage and database') {
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
          sh 'docker volume rm ${GROUP}.staging.${PROJECT}.storage'
          sh 'docker volume rm ${GROUP}.staging.${PROJECT}.database'
          echo 'Deleted old database and storage'
        }

      }
    }

    stage('Recreating database and storage volumes') {
      steps {
        sh 'docker volume create ${GROUP}.staging.${PROJECT}.storage'
        sh 'docker volume create ${GROUP}.staging.${PROJECT}.database'
        echo 'Created new volumes!'
      }
    }
      
    stage('Copying .env') {
      steps {
        sh 'cp .env.example .env'
      }
    }

    stage('Building and deploying') {
      steps {
        sh 'docker-compose --file ./docker/staging/docker-compose.yml up -d --build'
        echo 'Completed successfully!'
      }
    }

  }
  environment {
    GROUP = 'menke'
    PROJECT = 'testproject'
  }
}
