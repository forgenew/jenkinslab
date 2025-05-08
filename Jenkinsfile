pipeline {
    agent any

    environment {
        REMOTE_USER = 'yaop'                    // Имя пользователя на удалённой VM
        REMOTE_HOST = '127.0.0.1'            // IP-адрес удалённой VM
        SSH_CREDENTIALS_ID = 'ssh-key-jenkins'    // ID SSH-ключа, добавленного в Jenkins
        APACHE_LOG_PATH = '/var/log/apache2/access.log'
    }

    stages {
        stage('Install Apache on Remote VM') {
            steps {
                script {
                    echo "🛰️ Установка Apache2 на удалённой машине..."
                    sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                        sh """
                            ssh -o StrictHostKeyChecking=no $REMOTE_USER@$REMOTE_HOST \\
                            'sudo apt-get update && sudo apt-get install -y apache2'
                        """
                    }
                }
            }
        }

        stage('Check Apache Logs for Errors') {
            steps {
                script {
                    echo "📄 Анализ логов Apache на наличие ошибок 4xx и 5xx..."
                    sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                        sh """
                            ssh $REMOTE_USER@$REMOTE_HOST \\
                            "grep -E 'HTTP/1\\.[01]\" [45][0-9]{2}' $APACHE_LOG_PATH | tee apache_errors.log"
                        """
                    }
                }
            }
        }

        stage('Archive Error Logs') {
            steps {
                script {
                    echo "📦 Архивация логов ошибок..."
                    sh "mkdir -p logs"
                    sshagent (credentials: [SSH_CREDENTIALS_ID]) {
                        sh "scp $REMOTE_USER@$REMOTE_HOST:apache_errors.log logs/"
                    }
                    archiveArtifacts artifacts: 'logs/apache_errors.log', allowEmptyArchive: true
                }
            }
        }
    }

    post {
        always {
            echo '✅ Pipeline завершён.'
        }
    }
}
