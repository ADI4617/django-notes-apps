@Library("Shared") _

pipeline {
    agent { label "linux" }

    triggers {
        githubPush()
    }

    stages {
 
        stage('hey') {
            steps {
                script {
                    echo hey()
                }
            }
        }

        stage("Code Clone") {
            steps {
                script {
                    clone(
                    "https://github.com/ADI4617/django-notes-apps.git",
                    "main"
                )
                }
            }
        }

        stage("Code Build") {
            steps {
                script {
                    docker_build("notes-app","latest","adijenkins")
                }
            }
        }

        stage("Push to DockerHub") {
            steps {
                script{
                    docker_push("notes-app","latest","adijenkins")
                }
            }
        }

        stage("Deploy") {
            steps {
                sh '''
                    docker-compose down || true
                    docker rm -f $(docker ps -aq) || true
                    docker-compose up -d --build
                '''
            }
        }

    }
}
