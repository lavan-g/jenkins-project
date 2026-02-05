pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/lavan-g/jenkins-project.git'
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
                    ssh -o StrictHostKeyChecking=no ec2-user@54.242.231.72 "
                      sudo rm -rf /usr/share/nginx/html/*
                    "

                    scp -o StrictHostKeyChecking=no index.html \
                      ec2-user@54.242.231.72:/tmp/index.html

                    ssh -o StrictHostKeyChecking=no ec2-user@54.242.231.72 "
                      sudo mv /tmp/index.html /usr/share/nginx/html/index.html
                      sudo systemctl reload nginx
                    "
                    '''
                }
            }
        }
    }
}

