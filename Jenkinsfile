pipeline {
    agent any

    environment {
        REMOTE_HOST = '127.0.0.1'
        REMOTE_USER = 'yaop'
        SSH_CREDENTIALS_ID = 'ssh-key-jenkins'
    }

    stages {
        stage('Grant sudo to yaop') {
            steps {
                sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST \\
                        "echo '$REMOTE_USER ALL=(ALL) NOPASSWD:ALL' | sudo tee /etc/sudoers.d/90-jenkins-sudo && sudo chmod 440 /etc/sudoers.d/90-jenkins-sudo"
                    """
                }
            }
        }

        stage('Test sudo access') {
            steps {
                sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST \\
                        'sudo whoami'
                    """
                }
            }
        }
    }
}
