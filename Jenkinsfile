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

        stage('CODE ANALYSIS with SONARQUBE') {

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


post {
        
        success {
            // Mark the build as successful in GitHub using the GitHub Status API
            script {
                def commitSha = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()
                // 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
                // Replace 'your-job-name' with the name of your Jenkins job
                // Replace 'your-repo-owner' and 'your-repo-name' with the owner and name of your GitHub repository
                // Replace 'your-github-access-token' with a personal access token with 'repo' scope
                // See https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token
                withCredentials([string(credentialsId: 'github-connexion	', variable: 'GITHUB_TOKEN')]) {
                    httpRequest (
                        contentType: 'APPLICATION_JSON',
                        httpMode: 'POST',
                        authentication: 'none',
                        url: "https://api.github.com/repos/Oussama-gharbi/SpringApp/statuses/${commitSha}?access_token=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",
                        requestBody: '{"state": "success", "description": "Jenkins build passed", "context": "Jenkins/code-analysis"}'
                    )
                }




        }
        }
}
        }
        stage('Build Image') {
           steps {
                sh 'docker build -t ${IMAGETAG}/$DOCKER_IMAGE_NAME .'
            }
            
               }
         
        } 
            }
