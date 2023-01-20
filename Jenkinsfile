pipeline {
    agent {
        docker {
            image 'alpine:latest'
            args '-u root'
        }
    }
    environment {
        ANSIBLE_HOSTS = 'W3NlcnZlcnNdCnNlcnZlcjEgYW5zaWJsZV9ob3N0PTEwLjAuMi4xNQpbYWxsOnZhcnNdCmFuc2libGVfcHl0aG9uX2ludGVycHJldGVyPS91c3IvYmluL3B5dGhvbjMK'
    }
    stages {
        stage('Add SSH key') {
            steps {
                withCredentials([string(credentialsId: 'ansible-ssh-key', variable: 'SSH_KEY')]) {
                    sh 'mkdir -p ~/.ssh && chmod 700 ~/.ssh && echo $SSH_KEY | base64 -d > ~/.ssh/id_rsa && chmod 0400 ~/.ssh/id_rsa'
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
                sh 'echo $ANSIBLE_HOSTS | base64 -d > inventory.cfg'
                sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible -i inventory.cfg all -m ping -u root'
            }
        }
    }
}
