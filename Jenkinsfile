pipeline {
    agent any 
    
    stages{
        stage("Code"){
            steps {
                echo "cloning the code"
                git url: "https://github.com/dk5483/django-notes-app.git", branch: "main"
            }
        }
        stage("build"){
            steps{
                echo "building the image"
                sh "docker build -t django-note-app ."
            }
            
        }
        stage("Push to Docker hub"){
            steps {
                echo "pushing to docker hub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerPass", usernameVariable: "dockerUser")]) {
                    sh "docker tag django-note-app ${env.dockerUser}/django-note-app:latest"
                    sh "docker login -u ${env.dockerUser} -p '${env.dockerPass}'"
                    sh "docker push ${env.dockerUser}/django-note-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
            
        }
    }
}
