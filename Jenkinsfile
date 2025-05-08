pipeline {
    agent any

    environment {
        REMOTE_USER = 'yaop'
        REMOTE_PASS = '1'
        REMOTE_HOST = '127.0.0.1'  // ← ваша локальна VM
    }

    stages {
        stage('Login via password') {
            steps {
                sh """
                    sshpass -p "$REMOTE_PASS" ssh -o StrictHostKeyChecking=no \\
                    $REMOTE_USER@$REMOTE_HOST 'hostname && whoami'
                """
            }
        }
    }
}
