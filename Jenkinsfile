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
        script {
          // Using kubectl directly to deploy to Kubernetes
          kubernetesDeploy(configs: ['deployment.yaml', 'service.yaml'])
        }
      }
    }
  }
}
