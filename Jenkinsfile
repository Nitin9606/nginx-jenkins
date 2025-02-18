pipeline {
    agent any

    triggers {
        pollSCM('* * * * *') // Polls every minute for changes
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Nitin9606/nginx-jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("nginx-jenkins-image")
                }
            }
        }

         stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove any existing container with the same name
                    sh 'docker rm -f nginx-container || true'
                    // Run the new container
                    dockerImage.run('-p 8090:80 --name web')
    
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'docker system prune -f'
        }
    }
}
