pipeline {
    agent any
    
    stages{
        stage("clone code"){
            steps{
                git url: "https://github.com/farzshamim/django-notes-app" , branch : "main"
            }
        }
        stage("build and test"){
            steps{
                sh "docker build . -t note-todo"
            }
        }
        stage("push to docker hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag note-todo ${env.dockerHubUser}/note-todo:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/note-todo:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
