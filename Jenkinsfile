pipeline {
    agent { label "linux" }

    stages {

        stage("Checkout") {
            steps {
                checkout scm
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t notes-app:latest ."
            }
        }

        stage("Push to DockerHub") {
            steps {
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

        stage("Deploy") {
            steps {
                sh "docker-compose up -d --build"
            }
        }
    }
}
