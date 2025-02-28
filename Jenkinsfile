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
                sh "docker buil -t flask-app ."
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
              emailext from:'learnersubha0@gmail.com',
              to:'learnersubha0@gmail.com',
              body:'Good news: your build was successful',
              subject:'build successful'
          }
    }
    failure{
           script{
               emailext attachlog: true,
               from:'learnersubha0@gmail.com',
               to:'learnersubha0@gmail.com',
               body: 'Bad news: your build was failed',
               subject: 'build failure'
           }    
    }
}    
}
