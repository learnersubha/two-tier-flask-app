@Library("shared") _
pipeline {
    
    agent {label "dev"};
    
    stages {
        stage("code clone"){
            steps{
                script{
                    clone("https://github.com/learnersubha/two-tier-flask-app.git", "master")
                } 
            }
        }
        stage("trivy scan"){
            steps{
                script{
                    trivy()
                }    
            } 
            
        }
        stage("build") {
            steps {
                sh "docker build -t flask-app ."
            }
        }
        stage("push image in dockerhub") {
            steps {
               script{
                   docker_push("dockerhubauth","flask-app")
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
           success("learnersubha0@gmail.com","learnersubha0@gmail.com")
        }
    }
    failure{
        script{
           failure("learnersubha0@gmail.com","learnersubha0@gmail.com")         
        }
    }
}    
}
