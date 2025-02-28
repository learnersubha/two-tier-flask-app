@Library("shared") _
pipeline {
    agent {label "dev"};
    
    stages {
        stage("code clone") {
            script{
                clone("https://github.com/learnersubha/two-tier-flask-app.git","master")
            }
        }
        stage("trivy scan"){
            steps{
                sh "trivy fs . -o result.json"
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
    
post{
    success{
        script{
            emailext from:'learnersubha0@gmail.com',
        to:'learnersubha0@gmail.com',
        body: 'Godde news: your build was successful',
        subject: 'build successful'
        }
    }
    failure{
        script{
            emailext from:'learnersubha0@gmail.com',
        to:'learnersubha0@gmail.com',
        body: 'Bad news: your build was failed',
        subject: 'build failure'
        }
    }
}    
}
