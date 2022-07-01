def img
pipeline {
    environment {
        registry = "ecarmona1992/project1"
        registrycredential = "docker-hub-login"
        dockerimage = ''
    }
    agent any
    
    stages {
        
        stage("build checkout") {
            steps {
                echo 'Finshed downloading git'
            }
        }
        
        stage('Build Image') {
            steps {
                script {
                    img = registry + ":${env.BUILD_ID}"
                    println("${img}")
                    dockerImage = docker.build("${img}")
                }
            }
        }
        stage("Testing - running Jenkins Node") {
            steps {
                sh 'docker run -d --name $(JOB_NAME) -p 5000:5000 ${img}'
            }
        }
        
        stage("Push to DockerHub") {
            steps {
                script{
                    docker.withRegistry('https://registry.hub.docker.com', registryCredential) {
                    dockerImage.push()
                }
                }
            }
        }
    }
}