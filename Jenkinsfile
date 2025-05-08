pipeline {
    agent any

    environment {
        REMOTE_HOST = '127.0.0.1'
        REMOTE_USER = 'yaop'
        SSH_CREDENTIALS_ID = 'ssh-key-jenkins'
    }

    stages {
        stage('Ensure sudo access for yaop') {
            steps {
                sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                    // Створюємо sudo-правила, якщо ще не створені
                    sh """
                        ssh -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST '
                        echo "$REMOTE_USER ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/90-jenkins-sudo >/dev/null &&
                        sudo chmod 440 /etc/sudoers.d/90-jenkins-sudo'
                    """
                }
            }
        }

        stage('Verify sudo works') {
            steps {
                sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST \\
                        'sudo -n whoami | grep -q root'
                    """
                }
            }
        }

        stage('Install Apache on Remote VM') {
            steps {
                sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                    sh """
                        ssh -
