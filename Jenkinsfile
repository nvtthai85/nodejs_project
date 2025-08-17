pipeline {
    agent any
     
    tools {
        nodejs "Nodejs"
    }

    environment {
        DOCKER_HUB_REPO = "nguyenthai26/test_push"          // Thay bằng repo của bạn
    }

    stages {

        stage ('checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh 'node -v'
                sh 'npm -v'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

       /*  stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-node-app:2.0 .'  // Build image Docker
            }
        }      */ 

        stage ('Build Image'){
            steps {
               sh 'docker build -t ${DOCKER_HUB_REPO}:1.0 .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'testpush',
                        passwordVariable: 'DOCKER_PASSWORD',
                        usernameVariable: 'DOCKER_USERNAME'
                    )]) {
                        sh '''
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                            docker push ${DOCKER_HUB_REPO}:1.0
                            docker logout
                        '''
                    }
                }
            }
        }
    }
}