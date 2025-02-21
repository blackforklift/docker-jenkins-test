pipeline {  

    agent any    //where to execute


    environment {   //defining variables for code readablity
        DOCKER_IMAGE = "dilansas/flask-app:latest"
        COMPOSE_FILE = "compose.yaml"
    }


    stages { //stages where the work happens stages and steps

        stage('Checkout Code') {  

            steps {
               git credentialsId: 'github-credentials', url: 'https://github.com/blackforklift/docker-jenkins-test.git'
            }
        }

         stage("docker build") {  

            steps {
              sh 'docker build -t flask-redis-app .'
            }
        }
        stage("docker tag") {  

            steps {
              sh 'docker tag flask-redis-app $DOCKER_IMAGE'
            }
        }
       

        stage("docker push") {  
            steps {
                script {
                    withDockerRegistry([credentialsId: 'my-docker-hub-credentials', url: '']) {
                        sh 'docker push $DOCKER_IMAGE'
                    }
                }
            }
        }
       
        stage('docker pull') {
            steps {
                script {
                    sh 'docker pull $DOCKER_IMAGE'
                  
                }
            }
        }

        stage('docker run') {
            steps {
                script {
                sh 'docker-compose -f $COMPOSE_FILE up -d --force-recreate'
                }
            }
        }

    }

}