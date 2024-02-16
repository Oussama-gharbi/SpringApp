pipeline {
    agent any
    tools {
        maven 'maven'
    }
       stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo 'Pulling... ' + env.GIT_BRANCH
            }
        }

        
        stage('Test Code') {
            
            steps {
                script {
                    sh 'mvn test' 


                }
            }
        }
    }
}
