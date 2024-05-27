pipeline {
    agent any
    environment {
        IMAGE='liatrio/jenkins-alpine'
        TAG='latest'
    }
    stages {
        stage('Build') {
            steps {
                sh "docker build --pull -t ${IMAGE}:${TAG} ."
                sh "docker run -it -d  ${IMAGE}:${TAG}"
                sh "docker ps"
            }
        }
        stage('Push to dockerhub') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerPassword', usernameVariable: 'dockerUsername')]) {
                    sh "docker login -u ${env.dockerUsername} -p ${env.dockerPassword}"
                    sh "docker push ${env.IMAGE}:${TAG}"
                }
            }
        }
    }
}
