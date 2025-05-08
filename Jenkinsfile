pipeline {
    agent any

    environment {
        SSH_USER = 'yaop'
        SSH_HOST = '127.0.0.1'
    }

    stages {
        stage('Install Apache over SSH') {
            steps {
                sshagent(['ssh-key-jenkins']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${SSH_USER}@${SSH_HOST} << EOF
                            sudo apt-get update
                            sudo apt-get install -y apache2
                            sudo systemctl enable apache2
                            sudo systemctl start apache2
                        EOF
                    """
                }
            }
        }
    }
}
