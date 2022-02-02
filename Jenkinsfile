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

    stage('Cloning live volumes into staging') {
      steps {
        sh 'docker run --rm -i -t -v ${GROUP}.production.${PROJECT}.storage:/from -v ${GROUP}.staging.${PROJECT}.storage:/to alpine ash -c "cd /from; cp -av . /to"'
        sh 'docker run --rm -i -t -v ${GROUP}.production.${PROJECT}.database:/from -v ${GROUP}.staging.${PROJECT}.database:/to alpine ash -c "cd /from; cp -av . /to"'
        echo 'Duplicated volumes'
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
