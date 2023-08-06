pipeline {
    agent any

    stages {
        stage("Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/AntnioFiroz/django-notes-Jenkins.git", branch: "main"
            }
        }

        stage("Build") {
            steps {
                echo "Building the image"
                sh "docker build -t my-note-app-django ."
            }
        }

        stage("Push to docker hub") {
            steps {
                echo "Pushing the image to docker hub"
                 withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]){
                    sh "docker tag my-note-app-django ${env.dockerHubUser}/my-note-app-django:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/my-note-app-django:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
