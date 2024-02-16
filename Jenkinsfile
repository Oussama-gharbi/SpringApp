pipeline {
    agent any
    tools {
        maven 'maven'
        java 'jdk17'
    }
       stages {
        stage('Checkout') {
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
                
              
            }
        }
