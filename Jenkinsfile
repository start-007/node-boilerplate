pipeline {
  agent any

  stages {
    stage("init") {
      steps {
        echo "Initializing "
      }
    }
    environment{
      DOCKERHUB_REPO = "starteja007/node-app"

    }
    stage("versioning"){
      steps{
        script{
            def packageJson = readJSON file: 'package.json'
            def packageJsonVersion = packageJson.version
            env.IMAGE_VERSION=packageJsonVersion
            echo "${IMAGE_VERSION}"
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