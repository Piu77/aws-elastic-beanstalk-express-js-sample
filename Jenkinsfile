pipeline {
    agent any

    environment {
        SNYK_TOKEN = credentials('snyk-token')
        DOCKER_CREDS = credentials('docker-hub-creds')
    }

    stages {
        stage('Checkout Source') {
            steps {
                echo "Checking out repository..."
                checkout scm
                sh 'ls -l'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image..."
                sh 'docker build -t node-app:latest .'
            }
        }

        

        stage('Security Scan with Snyk') {
            steps {
                echo "Scanning Docker image for vulnerabilities..."
                sh '''
                  docker run --rm \
                    -e SNYK_TOKEN=$SNYK_TOKEN \
                    -v /var/run/docker.sock:/var/run/docker.sock \
                    snyk/snyk:docker snyk container test node-app:latest --severity-threshold=high || true
                '''
            }
        }

        
    }
}
