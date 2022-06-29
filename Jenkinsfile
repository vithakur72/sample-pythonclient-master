pipeline {
  agent any 
    environment {
        registryCredential = 'dockerhub'
        imageName = 'vithakur72/simplepy'
        dockerImage = ''
        }
    stages {
        
        stage('Building image') {
            steps {
                echo 'Retrieve source from github. run npm install and npm test' 
                git branch: 'main',
                    url: 'https://github.com/vithakur72/sample-pythonclient-master.git'
                echo 'repo files'
                sh 'ls -a'
                script {
                    echo 'build the image'
                    dockerImage = docker.build("${env.imageName}:${env.BUILD_ID}")
	                echo "${env.imageName}:${env.BUILD_ID}"
                    echo 'image built'
                }
            }
            }
         stage('Push Image') {
            steps{
                script {
                    echo 'push the image to docker hub'
                    docker.withRegistry('',registryCredential){
                        dockerImage.push("${env.BUILD_ID}")
                  }
                }
            }
        } 
        stage('deploy to k8s') {
             agent {
                docker { 
                    image 'google/cloud-sdk:latest'
                    args '-e HOME=/tmp'
                    reuseNode true
                        }
                    }
            steps {
                echo 'Get cluster credentials'
                sh 'gcloud container clusters get-credentials my-testing-cluster --zone us-west1-a --project capstone-352221'
                sh "kubectl set image deployment/sample-pythonclient-master sample-pythonclient-container=${env.imageName}:${env.BUILD_ID}"
              }
            }       
        stage('Remove local docker images') {
            steps{
                script {
                    echo 'push the image to docker hub' 
                }
                // sh "docker rmi $imageName:latest"
                sh "docker rmi $imageName:$BUILD_NUMBER"
            }
        }    
    }
    }
