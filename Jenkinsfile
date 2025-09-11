pipeline {
    agent any

    environment {
        IMAGE_NAME = "my-webapp"
    }

    stages {
        stage('Build') {
            steps {
                echo "Building Docker image for branch ${env.BRANCH_NAME}"
                script {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Test') {
            steps {
                echo "Running tests (placeholder)"
                // Insert real tests if any
                sh 'echo "Tests passed!"'
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'master'
            }
            steps {
                echo "Deploying to production"
                script {
                    sh '''
                    docker stop webapp || true
                    docker rm webapp || true
                    docker run -d -p 80:80 --name webapp $IMAGE_NAME
                    '''
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline complete for ${env.BRANCH_NAME}"
        }
    }
}
