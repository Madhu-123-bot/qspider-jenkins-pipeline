pipeline {
    agent any

    environment {
        // Path to the private key for Git
        SSH_KEY = '/var/lib/jenkins/.ssh/privatekey.pem'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Add SSH private key for Git operations
                    sshagent(credentials: []) {
                        sh """
                        eval \$(ssh-agent)
                        chmod 600 ${SSH_KEY}
                        ssh-add ${SSH_KEY}
                        git clone https://github.com/Madhu-123-bot/qspider-jenkins-pipeline.git
                        """
                    }
                }
            }
        }

        stage('Copy Files') {
            steps {
                script {
                    sh """
                    # Navigate to the Jenkins workspace where the repo was cloned
                    cd qspider-jenkins-pipeline
                    # Copy all the files to the /var/www/html/ directory
                    sudo cp -r * /var/www/html/
                    """
                }
            }
        }
    }

    post {
        always {
            // Cleanup SSH agent after the pipeline execution
            sh "killall ssh-agent || true"
        }
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
