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
            sh "git push $BUILD_NAME"
          }
        }
      }

    }
}
