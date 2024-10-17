pipeline {
    agent any

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('run-the-web')
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    bat "docker run -d --name changerpower run-the-web"
                }
            }
        }
                    
                    stage('Stop and Remove Existing Container') {
            steps {
                script {
                    def containerId = bat(script: 'docker ps -a -q -f name=changerpower', returnStdout: true).trim()
                    if (containerId) {
                        bat "docker stop changerpower"
                        bat "docker rm changerpower"
                    }
                    else {
                echo "No container named 'changerpower' found. Skipping stop and remove."
                    }
                }
            }
        }

        stage('Run New Docker Container') {
            steps {
                script {
                    bat "docker run -d --name changerpower -p 8081:80 run-the-web"
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
