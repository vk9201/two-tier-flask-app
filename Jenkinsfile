pipeline {
    agent any;
    stages {
        stage("Code"){
            steps{
              git url: "https://github.com/vk9201/two-tier-flask-app.git", branch: "master"
            }  
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."  
            }
        }
          stage("Push to Docker Hub"){
              steps{
                  withCredentials([usernamePassword(credentialsId:"DockerHubCreds",
                  passwordVariable: "DockerHubPass",
                  usernameVariable: "DockerHubUser"
                  )]){
                  sh "docker login -u ${env.DockerHubUser} -p ${DockerHubPass}"
                  sh "docker image tag two-tier-flask-app ${env.DockerHubUser}/two-tier-flask-app"
                  sh "docker push ${env.DockerHubUser}/two-tier-flask-app:latest "
                  }
              }
          }
        
        stage("Test"){
             steps{
                 echo " Developer/test karega..." 
             }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}

