pipeline {  

    agent any    //where to execute
    stages {

        stage("build") {  //stages where the work happens stages and steps

            steps {
                echo 'building the app'
            }
        }

         stage("test") {  

            steps {
                 echo 'testing the app'
            }
        }

         stage("deploy") { 

            steps {
                 echo 'deploying the app'
            }
        }

    }

}