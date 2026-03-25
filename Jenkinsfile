pipeline {
    agent any

    stages {

        stage('Checkout from GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Vaishnavibitla4/node-docker-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                docker build -t node-docker-app:%BUILD_NUMBER% .
                docker tag node-docker-app:%BUILD_NUMBER% docker.io/vaishnavibitla4/node-docker-app:%BUILD_NUMBER%
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat '''
                    docker login -u %DOCKER_USER% -p %DOCKER_PASS%
                    docker push docker.io/vaishnavibitla4/node-docker-app:%BUILD_NUMBER%
                    '''
                }
            }
        }
        
        stage('Create container') {
            steps {
                bat 'docker run -d -p 3000:8080 Vaishnavibitla4/node-docker-app:%BUILD_NUMBER%'
            }
        }



    }
}
