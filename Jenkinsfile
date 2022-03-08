pipeline {
  agent any
  stages {
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
      parallel {
        stage('Removing current container') {
          agent any
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh 'docker rm ${GROUP}.staging.${PROJECT}'
              echo 'Removed current staging container'
            }
          }
        }
        stage('Removing current containers database') {
          steps {
            catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
              sh 'docker rm ${GROUP}.staging.${PROJECT}.mysql'
              echo 'Removed mysql container'
            }
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
