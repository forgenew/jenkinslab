pipeline {
    agent any

    environment {
        REMOTE_HOST = '127.0.0.1'
        REMOTE_USER = 'ubuntu'
        REMOTE_PORT = '2222'
        SSH_CREDENTIALS_ID = '79ac7297-d402-4a98-b4e4-e825104d7a4f'
    }

    stages {
        stage('Connect via SSH') {
            steps {
                sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no -p $REMOTE_PORT $REMOTE_USER@$REMOTE_HOST \\
                        'echo З’єднання успішне!'
                    """
                }
            }
        }
    }
}
