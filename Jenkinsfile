pipeline {
    agent any
    tools {
        maven 'maven'
    }
       stages {
       /* stage('Checkout') {
            steps {
                checkout scm
                echo 'Pulling... ' + env.GIT_BRANCH
            }
        }
        
        stage('lint Test') {
            
            steps {
                script {
                    sh 'mvn checkstyle:checkstyle' 
                }
            }
        }
        
        stage('Unit Test') {
            
            steps {
                script {
                    sh 'mvn test' 
                }
            }
        }
        stage('integration Test') {
            
            steps {
                script {
                    sh 'mvn verify' 
                }
            }
            }
*/
        stage('Sonarqube Analysis') {
            steps {
            withSonarQubeEnv(credentialsId: 'sonarqube-token', installationName: 'sonar-server1') {
             sh' mvn sonar:sonar \
                    -Dsonar.projectKey=SpringApp \
                    -Dsonar.host.url=http://10.165.147.223:9000'
                    }
            }
                
        }  
            }
        }
