pipeline {
    agent any
    environment {
        IMAGE_NAME = "my-webapp"
    }
    options { timestamps() }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
            }
        }
        stage('Test Container') {
            steps {
                sh """
                   docker run --rm ${IMAGE_NAME}:${env.BRANCH_NAME}-${env.BUILD_NUMBER} \
                   sh -c 'echo Webapp OK'
                """
            }
        }
        stage('Deploy to Prod') {
            when { branch 'master' }
            steps {
                sh """
                   docker stop webapp || true
                   docker rm webapp || true
                   docker run -d --name webapp -p 80:80 \
                       ${IMAGE_NAME}:${env.BRANCH_NAME}-${env.BUILD_NUMBER}
                """
            }
        }
    }
    post {
        always { echo "Pipeline finished for ${env.BRANCH_NAME}" }
    }
}
