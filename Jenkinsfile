pipeline {
    agent any

    stages {
        stage('Build and Test') {
            steps {
                echo 'Building Docker Image...'
                bat 'docker build -t nginx-jenkins-image .'
            }
        }

        stage('Run Docker Container') {
            steps {
                echo 'Running Docker Container...'
                // Stop and remove the container if it already exists
                bat 'docker rm -f nginx-container || exit 0'
                // Run the container using the built image
                bat 'docker run -d -p 8090:80 --name nginx-container nginx-jenkins-image'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            bat 'docker system prune -f'
        }
    }
}
