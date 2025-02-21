pipeline {  

    agent any    //where to execute


    environment {   //defining variables for code readablity
        DOCKER_IMAGE = "dilansas/flask-app:latest"
        COMPOSE_FILE = "compose.yaml"
    }


    stages { //stages where the work happens stages and steps

        stage('Checkout Code') {  

            steps {
               git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/blackforklift/docker-jenkins-test.git'
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
        stage('cleanup prev containers') {
                    steps {
                        script {
                            sh 'docker-compose down'

                             sh 'docker ps'

                             sh 'docker rm -f redis || true'

                             sh 'docker rm -f redis || true'
                        
                        }
                    }
                }

        stage('docker run -redis') {
            steps {
                script {
                    sh 'docker network rm my_network || true '

                    sh'docker network create my_network'

                    sh 'docker run -d --name redis --network my_network -p 6379:6379 redislabs/redismod '
                }
            }
        }
        stage('docker run -app') {
            steps {
                script {
                sh 'echo "Current working directory: $PWD"'

                sh 'docker run -d --name flask_app --network my_network -p 8000:8000 -v -v "$PWD:/code" $DOCKER_IMAGE'
                }
            }
        }

    }

}