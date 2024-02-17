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
        
       /*   stage('lint Test') {
            
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
        stage('CODE ANALYSIS with SONARQUBE') {

            environment {
                scannerHome = tool 'sonar-scanner'
            }

            steps {
                withSonarQubeEnv('credentialsId: 'mytoken', installationName: 'sonar-server'') {
                    sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=SpringApp \
                   -Dsonar.projectName=SpringApp \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }

                // timeout(time: 10, unit: 'MINUTES') {
                //     waitForQualityGate abortPipeline: true
                // }
            }
        } 
            }
        }
