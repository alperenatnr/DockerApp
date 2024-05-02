pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/alperenatnr/DockerApp']]])
                bat 'mvn clean install'
            }
        }
        stage('Stop and Remove Existing Container') {
            steps {
                script {
                    bat 'docker stop DockerApp'
                    bat 'docker rm DockerApp'
                }
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    docker.build("demo12:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push image to Hub') {
            steps {
                script {
                    docker.image("demo12:${env.BUILD_NUMBER}").run("-d -p 4444:80 --name DockerApp")
                }
            }
        }
    }
}
