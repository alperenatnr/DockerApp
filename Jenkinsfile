pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'dockerapp'
        DOCKER_CONTAINER_NAME = 'DockerApp'
        DOCKER_PORT_MAPPING = '4444:80'
    }
    
    stages {
        stage('Build Maven') {
            tools {
                maven 'Maven' // Maven kurulumunu belirtin
            }
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'main']], userRemoteConfigs: [[url: 'https://github.com/alperenatnr/DockerApp']]])
                sh 'mvn clean install' // Maven projesini derle
            }
        }
        
        stage('Build Docker image') {
            steps {
                script {
                    docker.build("${env.DOCKER_IMAGE}:${env.BUILD_NUMBER}") // Docker imajını oluştur
                }
            }
        }
        
        stage('Run Docker container') {
            steps {
                script {
                    docker.image("${env.dockerapp}:${env.BUILD_NUMBER}").run("-d -p ${env.DOCKER_PORT_MAPPING} --name ${env.dockerapp}") // Docker konteynerını çalıştır
                }
            }
        }
    }
    
    post {
        always {
            // Jenkins işlemi bittiğinde her zaman çalışacak adımlar
            // Örneğin, Docker konteynerını durdurabiliriz
            script {
                docker.stop("${env.dockerapp}") // Docker konteynerını durdur
                docker.remove("${env.dockerapp}") // Docker konteynerını kaldır
            }
        }
    }
}
