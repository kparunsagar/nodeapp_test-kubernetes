pipeline {

  environment {
    DOCKERHUB_CREDENTIALS = credentials('docker')
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/kparunsagar/nodeapp_test-kubernetes.git'
      }
    }

    stage("Build docker image"){
      steps {
        sh 'docker build -t kparun/nodeapp:$BUILD_NUMBER .'
      }
    }
    stage("Login to docker hub"){
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage("Docker push"){
      steps {
        sh 'docker push kparun/nodeapp:$BUILD_NUMBER'
      }
    }
    stage('Deploying App to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deploymentservice.yml", kubeconfigId: "kubernetes")
        }
      }
    }

  }

}
