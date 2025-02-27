pipeline {
    agent any;
    
    stages {
        stage("code clone") {
            steps {
                git url: "https://github.com/learnersubha/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("build") {
            steps {
                sh "docker build -t flask-app ."
            }
        }
        stage("push image in dockerhub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerhubauth",
                    passwordVariable: "dockerhubpass",
                    usernameVariable: "dockerhubuser"
                )]) {
                    sh "echo ${env.dockerhubpass} | docker login -u ${env.dockerhubuser} --password-stdin"
                    sh "docker image tag flask-app ${env.dockerhubuser}/flask-app:latest"
                    sh "docker push ${env.dockerhubuser}/flask-app:latest"
                }
            }
        }
        stage("deploy") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
