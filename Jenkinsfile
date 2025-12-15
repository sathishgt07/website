pipeline {
    agent any

    environment {
        EC2_IP = "18.206.48.125"
    }

    stages {
        stage('Pull Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/sathishgt07/website'
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: ['ec2-ssh-key']) {
                    sh '''
                    scp -o StrictHostKeyChecking=no index.html ubuntu@$EC2_IP:/home/ubuntu/
                    ssh ubuntu@$EC2_IP "sudo mv /home/ubuntu/index.html /var/www/html/index.html && sudo systemctl reload nginx"
                    '''
                }
            }
        }
    }
}
