pipeline {
    agent any
    environment {
        IMAGE='FROM node:14'
        TAG='latest'
    }
    stages {
        stage('Checkout') {
           steps{ 
             checkout scmGit(branches: [[name: '*/master']], extensions: [], 
                             userRemoteConfigs: [[url: 'https://github.com/pushpen12/alpine-jenkins.git']])
                }
        }
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
