pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        SECRET_VAR = credentials('secret_text')
        DOCKER_IMAGE = "sahrmalik/nginx-custom:${BUILD_NUMBER}"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',  // Changed from 'master' to 'main'
                    credentialsId: 'git-credentials', 
                    url: 'https://github.com/SahrMalik/qa-jenkins-task5'
            }
        }
        
        stage('Echo Secret') {
            steps {
                sh 'echo $SECRET_VAR'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }
        
        stage('Login to DockerHub') {
            steps {
                script {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                script {
                    sh "docker push ${DOCKER_IMAGE}"
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    sh 'docker rm -f nginx-custom || true'
                    sh "docker run -d -p 80:80 --name nginx-custom ${DOCKER_IMAGE}"
                }
            }
        }
    }
    
    post {
        always {
            sh 'docker logout'
        }
    }
}