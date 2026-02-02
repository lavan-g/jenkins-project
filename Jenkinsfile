pipeline {
    agent { label 'agent-1' }

    options {
        skipDefaultCheckout(true)
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Balaji-official-006/jenkins-project.git'
            }
        }

        stage('Build') {
            steps {
                echo "Build step (static app)"
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['deploy-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ec2-user@10.0.15.212 << EOF
                      sudo rm -rf /usr/share/nginx/html/*
                      sudo cp -r * /usr/share/nginx/html/
                    EOF
                    '''
                }
            }
        }
    }
}
