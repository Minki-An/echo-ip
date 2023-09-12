pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/Minki-An/echo-ip.git', branch: 'main'
      }
    }
    stage('docker login') {
      steps {
        sh '''
        docker login --username=$DOCKER_USER --password=$DOCKER_PASS
        '''
      }
    }

    stage('docker build and push') {
      steps {
        sh '''
        docker build -t amk1513/echo-ip .
        docker push amk1513/echo-ip
        '''
      }
    }
    stage('deploy kubernetes') {
      steps {
        sh '''
        kubectl create deployment pl-bulk-prod --image=amk1513/echo-ip
        kubectl expose deployment pl-bulk-prod --type=NodePort --port=8080 \
                                               --target-port=80 --name=pl-bulk-prod-svc
        '''
      }
    }
  }
}
