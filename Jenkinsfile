pipeline {
    agent any

    environment {
        REMOTE_HOST = '127.0.0.1'  
    }

    stages {
        stage('Install Apache on Remote VM') {
            steps {
                withCredentials([usernamePassword(credentialsId: '79ac7297-d402-4a98-b4e4-e825104d7a4f', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                        sshpass -p "$PASS" ssh -o StrictHostKeyChecking=no \\
                        $USER@$REMOTE_HOST 'sudo apt update && sudo apt install -y apache2'
                    """
                }
            }
        }

        stage('Check Apache Logs for 4xx and 5xx') {
            steps {
                withCredentials([usernamePassword(credentialsId: '79ac7297-d402-4a98-b4e4-e825104d7a4f', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                        sshpass -p "$PASS" ssh -o StrictHostKeyChecking=no \\
                        $USER@$REMOTE_HOST "grep -E 'HTTP/1.1\\\\\" [45][0-9]{2}' /var/log/apache2/access.log || true"
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
