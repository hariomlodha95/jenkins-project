
pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t myapp:latest .'    
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                sh 'echo "No tests yet"'
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing image..."
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    sh 'docker tag myapp:latest $USERNAME/myapp:latest'
                    sh 'docker push $USERNAME/myapp:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying container..."
                sh '''
                docker rm -f myapp || true
                docker pull hariomlodha/myapp:latest
                docker run -d -p 3000:3000 --name myapp hariomlodha/myapp:latest
                '''
            }
        }

    }
}

