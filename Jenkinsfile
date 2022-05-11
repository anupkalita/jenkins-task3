pipeline{
    agent any

    environment{
      BUILD_NAME = "build-$currentBuild.number"
    }

    stages{

      stage('build docker image'){
        steps{
          sh "docker build -t anup10/jenkins-task3:$BUILD_NAME ."
        }
      }

      stage('tagging in git'){
        steps{
          sh "git tag $BUILD_NAME"
          withCredentials([gitUsernamePassword(credentialsId: 'github_creds', gitToolName: 'git-tool')]) {
            sh "git push --tags"
          }
        }
      }

      stage('pushing to docker hub'){
        steps{
          withCredentials([usernamePassword(credentialsId: 'dockerhub-cred-anupk', passwordVariable: 'passwordVar', usernameVariable: 'usernameVar')]) {
            sh "docker login --username $usernameVar --password $passwordVar"
          }
          sh "docker push anup10/jenkins-task3:$BUILD_NAME"
        }
      }

    }
}
