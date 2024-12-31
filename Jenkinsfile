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
    // trigger 3
    stage("versioning"){
      steps{
        script{
            sh "npm version patch --git-tag-version false"
            env.IMAGE_VERSION=readJSON(file: 'package.json').version
            echo "Version: ${IMAGE_VERSION}"

        }
      }
    }
    // stage("build") {
    //   steps {
    //     script{
    //         withCredentials([
    //                 usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USER', passwordVariable: 'PWD')
    //             ]) {
                
    //             sh "docker build -t ${DOCKERHUB_REPO}:${IMAGE_VERSION} ."
    //             sh "echo ${PWD} | docker login -u ${USER} --password-stdin"
    //             sh "docker push ${DOCKERHUB_REPO}:${IMAGE_VERSION}"

    //         }
    //     }
    //   }
    // }
    // stage("deploy") {
    //   steps {
    //     echo "Deploying to ec2"
    //   }
    // }
    
  }

  post {
   
    success {
        echo "Commiting new version to github"
        script{
          
          withCredentials([
              usernamePassword(credentialsId: 'github-credentials', usernameVariable: 'USER', passwordVariable: 'PWD')
          ]) {
            sh "git remote rm origin"
            sh "git remote add origin https://${USER}:${PWD}@github.com/start-007/node-boilerplate.git"
          }

          sh '''UPDATED_VERSION=$(node -p "require('./package.json').version")
                git remote rm origin
                git config --global user.email 'jenkins@example.com'
                git config --global user.name 'jenkins'
                git add package.json
                git add package-lock.json
                git commit -m "Updating service version from $CURRENT_VERSION to $UPDATED_VERSION" 
                git push origin $BRANCH_NAME '''
        }
    }

    failure {
      echo "Failure"
    }
  }
}