pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                sh 'test -f index.html'
                sh 'test -f Dockerfile'
                echo 'Application build successful'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'grep -q "Hello from Jenkins CI/CD!" index.html'
                echo 'Tests passed'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t jenkins-demo:latest .'
            }
        }

        stage('Deploy Application') {
            steps {
                sh 'docker rm -f jenkins-demo || true'
                sh 'docker run -d --name jenkins-demo -p 80:80 jenkins-demo:latest'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'curl -fsS http://localhost | grep "Hello from Jenkins CI/CD!"'
                echo 'Deployment verified successfully'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Build failed. Check Console Output.'
        }
    }
}
