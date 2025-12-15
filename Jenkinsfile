pipeline {
    agent any

    environment {
        EC2_IP = "18.206.48.125"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy to EC2') {
            steps {
                withCredentials([
                  sshUserPrivateKey(
                    credentialsId: 'ec2-ssh-key',
                    keyFileVariable: 'SSH_KEY',
                    usernameVariable: 'SSH_USER'
                  )
                ]) {
                    sh '''
                    chmod 600 $SSH_KEY
                    scp -o StrictHostKeyChecking=no -i $SSH_KEY index.html $SSH_USER@$EC2_IP:/home/$SSH_USER/
                    ssh -o StrictHostKeyChecking=no -i $SSH_KEY $SSH_USER@$EC2_IP "sudo mv /home/$SSH_USER/index.html /var/www/html/index.html && sudo systemctl reload nginx"
                    '''
                }
            }
        }
    }
}
