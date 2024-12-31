pipeline {
  agent any

  stages {
    stage("init") {
      steps {
        echo "Initializing "
      }
    }
    stage("build") {
      steps {
        echo "Building image"
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