pipeline {
  agent any

  stages {
    stage("build") {
      steps {
        echo "Building image"
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