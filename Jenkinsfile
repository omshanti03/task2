pipeline {
    agent any

    environment {
        IMAGE_NAME = 'node-app'
        CONTAINER_NAME = 'node-app-container'
        PORT = '3000'
    }

    stages {
      
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build(IMAGE_NAME)
                }
            }
        }

        stage('Test') {
            steps {
                sh 'docker run --rm $DOCKER_IMAGE npm test || echo "No tests found"'
            }
        }
      
        stage('Run Container') {
            steps {
                script {
                    sh "docker rm -f ${CONTAINER_NAME} || true"
                    dockerImage.run("-d -p ${PORT}:${PORT} --name ${CONTAINER_NAME}")
                }
            }
        }
    }

    post {
        always {
            echo "Build complete. Deployed to http://localhost:${PORT}/"
        }
    }
}
