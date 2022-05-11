pipeline{
    agent any

    environment{
      BUILD_NAME = "build-$currentBuild.number"
    }

    stages{

      stage('build docker image'){
        steps{
          sh "docker build -t nagarajan2312/jenkins_assignment_10:$BUILD_NAME ."
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

      stage('pushing to docker hub'){
        steps{
          withCredentials([usernamePassword(credentialsId: 'dockerhub_credentials', passwordVariable: 'passwordVar', usernameVariable: 'usernameVar')]) {
            sh "docker login --username $usernameVar --password $passwordVar"
          }
          sh "docker push nagarajan2312/jenkins_assignment_10:$BUILD_NAME"
        }
      }

    }

    post{
      success{
        build job: 'assignment10_another_pipeline',parameters:[string(name: 'build_number',value: "$currentBuild.number")]
      }
    }
}
