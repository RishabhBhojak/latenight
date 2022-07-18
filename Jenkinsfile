pipeline {
  agent any
  environment {
        DOCKERHUB_CREDENTIALS_PSW="f1d00111-ecbc-48ed-b5cf-8d99e2a9e5d4"
        DOCKERHUB_CREDENTIALS_USR="rishabhbhojak" 
        
  }      
  stages {
    stage("verify tooling") {
      steps {
        sh '''
          docker version
          docker info
          docker-compose --version 
          curl --version
          
        '''
      }
    }
    stage('Prune Docker data') {
      steps {
        sh 'docker system prune -a --volumes -f'
      }
    }
    stage('kill port') {
      steps {
        sh 'alias kill3000="fuser -k -n tcp 3000"'
        sh 'alias kill80="fuser -k -n tcp 80"'
         
      }
    }
    stage('Build') {
      steps {
        sh ' docker build -t rishabhbhojak/mynodeimagefresh .' 
      }
    }  

    stage('login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }

    stage('push images') {
      steps {
        sh 'docker push rishabhbhojak/mynodeimagefresh'
      }
    }

    stage('deployment') {
      steps {
        sh 'oc login --token=sha256~BlDJKoKJRnrhOjj8IZJhituU26k1aqR5O8eijNZzdkw --server=https://c115-e.us-south.containers.cloud.ibm.com:32528'
        sh 'oc new-project myloadaaplication'
        sh 'oc project myloadaaplication'
        sh 'oc apply -f application'
        
      }
    




    }

  }
  
}
