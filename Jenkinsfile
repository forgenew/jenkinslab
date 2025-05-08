pipeline {
    agent any

    environment {
        REMOTE_HOST = '127.0.0.1'
        REMOTE_USER = 'ubuntu'
        REMOTE_PORT = '2222'
        SSH_CREDENTIALS_ID = 'ssh-key-jenkins'
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
