pipeline {
    agent any

    environment {
        EC2_IP = '3.21.100.35'
        USER = 'ubuntu'
        KEY_PATH = '/var/lib/jenkins/.ssh/privatekey.pem' // Updated path on Jenkins EC2
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Madhu-123-bot/qspider-jenkins-pipeline.git'
            }
        }
	
        stage('Deploy to NGINX Server') {
            steps {
                sh """
                ssh -i $KEY_PATH -o StrictHostKeyChecking=no ${USER}@${EC2_IP} 'rm -rf /var/www/html/*'
                scp -i $KEY_PATH -o StrictHostKeyChecking=no -r * ${USER}@${EC2_IP}:/var/www/html/
                """
            }
        }
    }

    post {
        always {
            echo 'Deployment completed!'
        }
    }
}
