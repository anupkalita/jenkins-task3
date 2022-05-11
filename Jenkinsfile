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
          sshagent (credentials: ['github_credentials']) {
            sh "git push --tags"
          }
        }
      }

    }

}
