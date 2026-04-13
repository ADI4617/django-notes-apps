pipeline {
    agent { label "linux" }

    triggers {
        githubPush()
    }

    stages{

        stage("Code clone"){
            steps{
                sh "whoami"
                git branch: 'main', url: 'https://github.com/LondheShubham153/django-notes-app.git'
            }
        }

        stage("Code Build"){
            steps{
                sh "docker build -t notes-app:latest ."
            }
        }

        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCred',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker tag notes-app:latest $DOCKER_USER/notes-app:latest
                    docker push $DOCKER_USER/notes-app:latest
                    '''
                }
            }
        }

        stage("Deploy"){
            steps{
                sh "docker-compose up -d --build"
            }
        }
    }
}
