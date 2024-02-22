pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment{
    IMAGETAG = "10.165.147.221" 
    DOCKER_IMAGE_NAME = "devops-project"

        
    }
       stages {
  /*  stage('Checkout') {
            steps {
                checkout scm
                echo 'Pulling... ' + env.GIT_BRANCH
            }
        }*/
        
       /*  stage('lint Test') {
            
            steps {
                script {
                    sh 'mvn checkstyle:checkstyle' 
                }
            }
        }*/
        
       /* stage('Unit Test') {
            
            steps {
                script {
                    sh 'mvn test' 
                }
            }
        }*/
      /*  stage('integration Test') {
            
            steps {
                script {
                    sh 'mvn verify' 
                }
            }
            }*/

       /* stage('CODE ANALYSIS with SONARQUBE') {

            environment {
                scannerHome = tool 'sonar-scanner'
                       }
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=SpringApp \
                   -Dsonar.projectName=SpringApp \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ 
                   '''
                }

                // timeout(time: 10, unit: 'MINUTES') {
                //     waitForQualityGate abortPipeline: true
                // }
            }

}*/
        
            /*    stage('Build Image') {
           steps {
                sh 'docker build -t ${IMAGETAG}/$DOCKER_IMAGE_NAME .'
            }
            
               }*/
         
        } 
}
