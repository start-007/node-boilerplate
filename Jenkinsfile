pipeline {
  agent any
  environment {
      DOCKERHUB_REPO = "starteja007/node-app"

  }
  stages {
    stage("init") {
      steps {
        echo "Initializing "
      }
    }
    // trigger
    stage("versioning"){
      steps{
        script{
            sh "npm version patch --git-tag-version false"
            env.IMAGE_VERSION=readJSON(file: 'package.json').version
            echo "Version: ${IMAGE_VERSION}"

        }
      }
    }
    stage("build") {
      steps {
        script{
            withCredentials([
                    usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USER', passwordVariable: 'PWD')
                ]) {
                
                sh "docker build -t ${DOCKERHUB_REPO}:${IMAGE_VERSION} ."
                sh "echo ${PWD} | docker login -u ${USER} --password-stdin"
                sh "docker push ${DOCKERHUB_REPO}:${IMAGE_VERSION}"

            }
        }
      }
    }
    stage("deploy") {
      steps {
        echo "Deploying to ec2"
      }
    }
    
  }

  post {
   
    success {
        echo "Successs"
    }

    failure {
      echo "Failure"
    }
  }
}