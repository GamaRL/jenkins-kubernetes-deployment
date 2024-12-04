pipeline {
  agent any

  environment {
    dockerimagename = "gamarl01/react-app"
  }

  stages {
    stage('Checkout Source') {
      steps {
        checkout([$class: 'GitSCM', 
                  branches: [[name: '*/main']], 
                  userRemoteConfigs: [[url: 'https://github.com/GamaRL/jenkins-kubernetes-deployment.git']]
        ])
      }
    }

    stage('Build image') {
      steps {
        script {
          def dockerImage = docker.build(dockerimagename)
        }
      }
    }

    stage('Deploying App to Kubernetes') {
      steps {
        sh 'kubectl apply -f deployment.yaml'
        sh 'kubectl apply -f service.yaml'
      }
    }
  }
}
