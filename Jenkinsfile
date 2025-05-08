pipeline {
    agent any

    environment {
        REMOTE_HOST = '127.0.0.1'   // ← Ваша локальна IP-адреса
        REMOTE_USER = 'ubuntu'           // ← Користувач на віддаленому сервері
        SSH_CREDENTIALS_ID = 'ssh-key-jenkins'
    }

    stages {
        stage('Ping Remote Server') {
            steps {
                script {
                    echo "🔗 Пінгуємо $REMOTE_HOST..."
                    sh "ping -c 2 $REMOTE_HOST"
                }
            }
        }

        stage('SSH to Remote Server') {
            steps {
                sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST \\
                        'hostname && uptime'
                    """
                }
            }
        }
    }
}
