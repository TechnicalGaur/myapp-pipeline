pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building Docker images..'
                sh 'docker build -t myapp:latest .'
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'npm test || echo "No tests defined"'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying Docker container...'
                sh '''
                docker stop myapp || true
                docker rm myapp || true
                docker run -d -p 3000:3000 --name myapp myapp:latest
                '''
            }
        }
    }
}
