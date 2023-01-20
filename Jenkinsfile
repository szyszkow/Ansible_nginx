pipeline {
    agent {
        docker {
            image 'alpine:latest'
            args '-u root'
        }
    }
    stages {
        stage('Add SSH key') {
            steps {
                withCredentials([string(credentialsId: 'ansible-ssh-key', variable: 'SSH_KEY')]) {
                    sh 'set +x && mkdir -p ~/.ssh && chmod 700 ~/.ssh && echo $SSH_KEY | base64 -d > ~/.ssh/id_rsa && chmod 0400 ~/.ssh/id_rsa'
                }
            }
        }
        stage('Install Ansible') {
            steps {
                sh 'apk add ansible'
                sh 'ansible --version'
            }
        }
        stage('Install nginx') {
            steps {
                sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible -i hosts all -m ping -u root'
            }
        }
    }
}
