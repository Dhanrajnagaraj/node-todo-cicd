pipeline {
    agent any
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/dhanrajnagaraj/node-todo-cicd.git", branch: "master"
                echo 'code clone '
            }
        }
        stage("build and test"){
            steps{
                sh "docker build -t node-app-demo ."
                echo 'code build is done'
            }
        }
        stage("scan image"){
            steps{
                echo 'image scanning'
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app-demo:latest ${env.dockerHubUser}/node-app-demo:latest"
                sh "docker push ${env.dockerHubUser}/node-app-demo:latest"
                echo 'image push to docker hub'
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo 'deployment'
            }
        }
    }
}
