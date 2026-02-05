pipeline {
    agent any

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
                    # Ensure target directory exists
                    ssh -o StrictHostKeyChecking=no ec2-user@10.0.15.212 \
                      "sudo mkdir -p /usr/share/nginx/html"

                    # Copy files from Jenkins agent to EC2
                    scp -o StrictHostKeyChecking=no -r ./* \
                      ec2-user@10.0.15.212:/tmp/webapp

                    # Move files into nginx directory
                    ssh -o StrictHostKeyChecking=no ec2-user@10.0.15.212 \
                      "sudo rm -rf /usr/share/nginx/html/* && sudo cp -r /tmp/webapp/* /usr/share/nginx/html/"
                    '''
                }
            }
        }
    }
}
