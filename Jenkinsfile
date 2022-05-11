pipeline{
    agent any

    environment{
      BUILD_NAME = "build-$currentBuild.number"
    }

    stages{

      stage('build docker image'){
        steps{
          sh "docker build -t jenkins-task3:$BUILD_NAME ."
        }
      }

      stage('tagging in git'){
        steps{
          sh "git tag $BUILD_NAME"
          withCredentials([gitUsernamePassword(credentialsId: 'github_creds', gitToolName: 'git-tool')]) {
            sh "git push $BUILD_NAME HEAD:master"
          }
        }
      }

      stage('pushing to docker hub'){
        steps{
          sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
          sh "docker push anup10/jenkins-task3:$BUILD_NAME"
          sh 'docker logout'
        }
      }

    }
}
