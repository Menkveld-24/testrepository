pipeline {
  agent any
  stages {
    stage('Stopping current container') {
      parallel {
        stage('Stopping current container') {
          agent any
          steps {
            sh '''docker stop ${GROUP}.staging.${PROJECT}
'''
            echo 'Stopped current staging container'
          }
        }

        stage('Stopping current containers database') {
          steps {
            sh '''docker stop ${GROUP}.staging.${PROJECT}.mysql
'''
            echo 'Stopped mysql container'
          }
        }

      }
    }

    stage('Deleting current staging containers') {
      steps {
        sh 'docker rm ${GROUP}.staging.${PROJECT}'
        sh 'docker rm ${GROUP}.staging.${PROJECT}.mysql'
        echo 'Deleted old containers'
      }
    }

    stage('Removing staging storage and database') {
      steps {
        sh 'docker volume rm ${GROUP}.staging.${PROJECT}.storage'
        sh 'docker volume rm ${GROUP}.staging.${PROJECT}.database'
        echo 'Deleted old database and storage'
      }
    }

    stage('Recreating database and storage volumes') {
      steps {
        sh 'docker volume create ${GROUP}.staging.${PROJECT}.storage'
        sh 'docker volume create ${GROUP}.staging.${PROJECT}.database'
        echo 'Created new volumes!'
      }
    }
    
    stage('Freezing production containers') {
      steps {
        sh 'docker pause ${GROUP}.production.${PROJECT}'
        sh 'docker pause ${GROUP}.production.${PROJECT}.mysql'
        echo 'Froze containers'
      }
    }

    stage('Cloning live volumes into staging') {
      steps {
        catchError(buildResult: 'SUCCESS', stageResult: 'FAILED') {
            sh 'docker_clone_volume.sh ${GROUP}.production.${PROJECT}.storage ${GROUP}.staging.${PROJECT}.storage'
            sh 'docker_clone_volume.sh ${GROUP}.production.${PROJECT}.database ${GROUP}.staging.${PROJECT}.database'
            echo 'Duplicated volumes'
        }
      }
    }
    
    stage('Resume containers') {
      steps {
        sh 'docker unpause ${GROUP}.production.${PROJECT}.mysql'
        sh 'docker unpause ${GROUP}.production.${PROJECT}'
        echo 'Resumed containers'
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
    GROUP = 'iea'
    PROJECT = 'testlaravel'
  }
}
