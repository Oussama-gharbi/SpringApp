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
             sh " mvn sonar:sonar  sh "mvn sonar:sonar -Dintegration-tests.skip=true -Dmaven.test.failure.ignore=true"
                    }
            }
                
        }  
            }
        }
