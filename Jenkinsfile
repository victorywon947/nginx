pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/IaC-Source/echo-ip.git', branch: 'main'
      }
    }
      stage('docker build') {
      steps {
        sh '''
        
        docker build -t multi-img .
        docker tag multi-img 192.168.0.53:5000/multi-img
        docker push 192.168.0.53:5000/multi-img
        '''
      }
    }
    stage('deploy kubernetes') {
      steps {
        sh '''
        kubectl create deployment pl-bulk-prod --image=192.168.0.53:5000/multi-img
        kubectl expose deployment pl-bulk-prod --type=LoadBalancer --port=8080 \
                                               --target-port=80 --name=pl-bulk-prod-svc
        '''
      }
    }
  }
}
