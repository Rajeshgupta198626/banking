pipeline {
     agent any

	environment {	
		DOCKERHUB_CREDENTIALS=credentials('dockerhubpwd')
	}
		
    stages {
        stage('SCM_Checkout') {
            steps {
                echo 'Perform SCM Checkout'
                git 'https://github.com/rajeshsonu2025/banking.git'
            }
        }
        stage('Application Build') {
            steps {
                echo 'Perform Application Build'
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                echo 'Perform Docker Build'
				sh "docker build -t rajeshyadavhub/bankapp-eta-app:${BUILD_NUMBER} ."
				sh "docker tag rajeshyadavhub/bankapp-eta-app:${BUILD_NUMBER} rajeshyadavhub/bankapp-eta-app:latest"
				sh 'docker image list'
            }
        }
        stage('Login to Dockerhub') {
            steps {
                echo 'Login to DockerHub'				
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                
            }
        }
        stage('Publish the Image to Dockerhub') {
            steps {
                echo 'Publish to DockerHub'
				sh "docker push rajeshyadavhub/bankapp-eta-app:latest"                
            }
        }
        stage('Deploy to test server') {
            steps {
				script {
		        ansiblePlaybook become: true, credentialsId: 'ansible2', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts/', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
			}				          
            }
        }
    }
}
