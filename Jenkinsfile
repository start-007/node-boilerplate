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
    stage("build") {
      steps {
        script{
            withCredentials([
                    usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USER', passwordVariable: 'PWD')
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
        script{
          echo "Deploying to ec2"
          def dockercmd= 'docker run -p 3000:3000 -d starteja007/node-app:1.0.1'
         

          sshagent(['ec2-ssh-key']){
            
            sh "ssh -o StrictHostKeyChecking=no ubuntu@ec2-3-87-0-247.compute-1.amazonaws.com ${dockercmd}"
          }
        }
      }
    }
    
  }

  post {
   
    success {
        echo "Commiting new version to github"
        // script{
          
          
        //             withCredentials([
        //       sshUserPrivateKey(credentialsId: 'github-ssh', keyFileVariable: 'SSH_KEY')
        //   ]) {
        //       // Set the SSH key for git authentication
        //       sh """
        //           mkdir -p ~/.ssh
        //           echo "$SSH_KEY" > ~/.ssh/id_ed25519
        //           chmod 600 ~/.ssh/id_ed25519
        //       """

        //       // Configure Git user
        //       sh "git config user.email 'admin@example.com'"
        //       sh "git config user.name 'example'"

        //       // Ensure you're on the correct branch (e.g., main or master)
        //       // Change 'main' to your desired branch name if different

        //       // Add changes and commit
        //       sh "git add ."
        //       sh "git commit -m 'Triggered Build changed version'"

        //       // Push the changes using SSH
        //       sh "git push git@github.com:start-007/node-boilerplate.git HEAD:master"
        //   }

        //}
    }

    failure {
      echo "Failure"
    }
  }
}