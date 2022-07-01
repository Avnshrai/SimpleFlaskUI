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
                    println ("${img}")
                    dockerImage = docker.build("${img}")
                }
            }
        }



        stage('Test - Run Docker Container on Jenkins node') {
           steps {

                sh label: '', script: "docker run -d --name ${JOB_NAME} -p 5000:5000 ${img}"
          }
        }

        stage('Push To DockerHub') {
            steps {
                script {
                    docker.withRegistry( 'https://registry.hub.docker.com ', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        
    }
}