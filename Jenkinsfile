pipeline {
  environment {
    imagename = "yenigul/hacicenkins"
    registryCredential = 'yenigul-dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/ismailyenigul/hacicenkins.git', branch: 'master', credentialsId: 'ismailyenigul-github-user-token'])

      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Test Image') {
        when {
      
          anyOf {
            branch 'test'
          }
       
     }
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("test-$BUILD_NUMBER")
             dockerImage.push('test-latest')

          }
        }
      }
    }
stage('Deploy Master Image') {
   when {
      anyOf {
            branch 'master'
      }
     }
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }

 stage('Remove Unused docker image - test') {
      when {
      anyOf {
            branch 'test'
      }
     }
      steps{
        sh "docker rmi $imagename:test-$BUILD_NUMBER"
         sh "docker rmi $imagename:test-latest"

      }
    } // End of remove unused docker image for master
    stage('Remove Unused docker image - Master') {
      when {
      anyOf {
            branch 'master'
      }
     }
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"

      }
    } // End of remove unused docker image for master
  }  
} //end of pipeline
