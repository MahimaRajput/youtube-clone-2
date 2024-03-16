pipeline {
    environment {
        DOCKER_CRED_ID = 'Docker_id' 
    }
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'Node16'
    }
    parameters {
        choice(
            name: 'action',
            choices: ['create', 'update'],
            description: 'Select action to perform'
        )
    }
    stages {
        stage('git checkout') {
            steps {
                git branch: 'master',
                credentialsId: '0f5a239d-86ca-4a61-bad1-a36f80b67ac6',
                url: 'https://github.com/MahimaRajput/Youtube-clone-2'
            }
        }
        stage('Npm') {
            when {
                expression { params.action == 'create' }
            }
            steps {
                script {
                    // You can execute npmInstall() or any npm related commands here
                    sh "npm run start"
                    echo 'Running npmInstall...'
                }
            }
        }
	    stage('Docker Build & Push') 
		{
			steps 
			{
			    script
			    {
                  withDockerRegistry([url: 'https://index.docker.io/v1/', credentialsId: 'Docker_id']){
                        sh "docker build -t mahimarajput26/youtube_clone2:${BUILD_NUMBER} ."
				        sh "docker build -t mahimarajput26/youtube_clone2:latest ."
					    sh "docker push mahimarajput26/youtube_clone2:${BUILD_NUMBER}"
					    sh "docker push mahimarajput26/youtube_clone2:latest"	
					}
			    }
			}
		}
        stage('Deploy to container'){
            steps{
                sh 'docker run -d --name youtube1 -p 3000:3000  mahimarajput26/youtube_clone2:latest'
            }
        }
    }
}
