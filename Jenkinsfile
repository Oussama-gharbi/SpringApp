pipeline {
    agent any
    tools {
        maven 'maven'
    }
    environment{
   
    NEXUS_URL = '10.165.147.221:8083' // Nexus Repository Manager URL
    NEXUS_CREDENTIALS_ID = 'nexus-cred' // Jenkins credentials ID for Nexus authentication
    DOCKER_IMAGE_NAME = "devops-project" // Docker image name
   DOCKER_IMAGE_TAG = 'v5.0' // Docker image tag
    DOCKER_IMAGE_FULL_NAME = "${NEXUS_URL}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}" // Full Docker image name including tag
    DOCKER_IMAGE_REPO = 'docker-hub' // Nexus Docker repository name
        
    }
       stages {
      stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
             
      stage('Checkout') {
            steps {
                checkout scm
                echo 'Pulling... ' + env.GIT_BRANCH
            }
        }
            stage("increment version") {
            steps {
                script {
                    echo 'incrementing app version...'
                    sh "sudo mvn build-helper:parse-version versions:set -DnewVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.nextIncrementalVersion} versions:commit"
                  // def matcher = readFile('pom.xml') =~ /<version>(.*)<\/version>/
                  // def version = matcher[0][1]
                  //env.DOCKER_IMAGE_TAG = env.DOCKER_IMAGE_TAG + "$version-$buildNumber"
                }
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
/*
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

}*/
        
                stage('Build Image') {
           steps {
                sh 'docker build -t ${DOCKER_IMAGE_FULL_NAME} . '
                 sh 'echo ${DOCKER_IMAGE_FULL_NAME} '
            }
            
               }

         stage('Docker Build and Push to Nexus') {
            steps {
                script {
                    
                        withCredentials([usernamePassword(credentialsId: "${NEXUS_CREDENTIALS_ID}", usernameVariable: 'USER', passwordVariable: 'PASSWORD')]){
                        sh 'echo $PASSWORD | docker login -u $USER --password-stdin $NEXUS_URL'
                        //sh 'docker system prune -af'
                        sh "docker build -t $DOCKER_IMAGE_FULL_NAME ."
                        sh "docker push $DOCKER_IMAGE_FULL_NAME"
                    }
                }
            }
        }

    stage('Deploy App in testserver'){
          steps {
             script{
             sshagent(['secret_key']) {
            
            sh '''
                 ssh -o StrictHostKeyChecking=no  user-ansible@10.165.147.122 sudo sed -i "s/:v[0-9]\\.[0-9]/:${DOCKER_IMAGE_TAG}/g" /etc/ansible/hosts
                 ssh -o StrictHostKeyChecking=no  user-ansible@10.165.147.122 "ansible-playbook /etc/ansible/run_docker.yml"'''
}
                   
             }
                            
          }

          }
    stage('commit version update'){
        steps{
            script{
                withCredentials([usernamePassword(credentialsId: 'github-cred', passwordVariable: 'PASS', usernameVariable: 'USER')] )
                
                sh 'git remote set-url origin https://${USER}:${PASS}github.com/Oussama-gharbi/SpringApp.git'
                sh 'git add . '
                sh 'git commit -m "ci: version pump"'
                sh 'git  push origin HEAD:develop'
        }
    }
           
}
}
}
