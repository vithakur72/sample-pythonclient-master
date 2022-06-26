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
    }
    }
