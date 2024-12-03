pipeline {

  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: docker
            image: docker:latest
            command:
            - cat
            tty: true
            volumeMounts:
             - mountPath: /var/run/docker.sock
               name: docker-sock
          volumes:
          - name: docker-sock
            hostPath:
              path: /var/run/docker.sock    
        '''
    }
  }

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
          kubernetesDeploy(configs: ['deployment.yaml', 'service.yaml'])
        }
      }
    }

  }

}
