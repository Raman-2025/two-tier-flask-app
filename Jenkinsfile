pipeline{
  agent{label 'vinod'}
  stages{
    stage('code'){
      steps{
        git url:"https://github.com/Raman-2025/two-tier-flask-app", branch:"master"
        echo "code clone successful"
      }
    }
    stage('build'){
      steps{
        sh "docker build -t two-tier-app ."
        echo "image build successful"
      }
    }
    stage('push to dockerhub'){
      steps{
        withCredentials([usernamePassword(
          credentialsId:"dockerHubCred",
          usernameVariable:"dockerHubUser",
          passwordVariable:"dockerHubPass"
        )]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
          sh "docker image tag two-tier-app  ${env.dockerHubUser}/two-tier-app "
          sh "docker push  ${env.dockerHubUser}/two-tier-app  "
        }
      }
    }
    stage('deploy'){
      steps{
        sh "docker compose down && docker compose up -d --build"
      }
    }    
  }
}
