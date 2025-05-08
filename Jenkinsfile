pipeline {
    agent any

    environment {
        REMOTE_HOST = '127.0.0.1'           
        REMOTE_USER = 'yaop'                    
        SSH_CREDENTIALS_ID = 'ssh-key-jenkins1'   
    }

    stages {
        stage('Install Apache on Remote VM') {
            steps {
                sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST \\
                        'apt update && apt install -y apache2'
                    """
                }
            }
        }

        stage('Check Apache Logs for 4xx and 5xx') {
            steps {
                sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST \\
                        "grep -E 'HTTP/1.1\\\\\" [45][0-9]{2}' /var/log/apache2/access.log || true"
                    """
                }
            }
        }
    }

    post {
        always {
            echo '✅ Pipeline завершено.'
        }
    }
}
