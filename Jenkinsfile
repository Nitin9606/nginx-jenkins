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
        
        stage("Build and Test") {
            steps {
                echo "Building Docker Image.."
                sh "docker build -t nginx-jenkins-image ."
            }
        }

        stage('Run Docker Container') {
            steps {
                echo "Running Docker Container.."
                // Stop and remove any existing container with the same name
                sh 'docker rm -f web || true'
                // Run the new container using built image
                sh "docker run -dit --name web -p 8090:80 nginx-jenkins-image"
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
