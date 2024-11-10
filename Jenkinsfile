pipeline {
     agent any

	environment {	
		DOCKERHUB_CREDENTIALS=credentials('dockerhubpwd')
	}
		
    stages {
        stage('SCM_Checkout') {
            steps {
                echo 'Perform SCM Checkout'
                git 'https://github.com/Rajeshgupta198626/banking.git'
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
				sh "docker build -t rajeshguptahub/bankapp-eta-app:${BUILD_NUMBER} ."
				sh "docker tag rajeshguptahub/bankapp-eta-app:${BUILD_NUMBER} rajeshguptahub/bankapp-eta-app:latest"
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
				sh "docker push rajeshguptahub/bankapp-eta-app:latest"                
            }
        }
        stage('Deploy to test server') {
            steps {
				script {
		        ansiblePlaybookansiblePlaybook become: true, credentialsId: 'Ansible', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'ansible-playbook.yml', vaultTmpPath: ''
			}				          
            }
        }
    }
}
