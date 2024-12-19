pipeline {
    agent any

        environment {
        AWS_ACCESS_KEY_ID = credentials('amazonlinux')  // ID from Jenkins credentials store
        AWS_SECRET_ACCESS_KEY = credentials('amazonlinux')  // ID from Jenkins credentials store
        AWS_DEFAULT_REGION = 'us-east-1'
        }
   stages {
        stage('Checkout') {
            steps {
                deleteDir()
                sh 'echo cloning repo'
                sh 'git clone https://github.com/rarvez77/ansible-task.git' 
            }
        }
        
        stage('Terraform Apply') {
            steps {
                script {
                    dir('/var/lib/jenkins/workspace/Challenge/ansible-task') {
                    sh 'pwd'
                    sh 'terraform init'
                    sh 'terraform validate'
                    // sh 'terraform destroy -auto-approve'
                    sh 'terraform plan'
                    sh 'terraform apply -auto-approve'
                    }
                }
            }
        }
        
        stage('Ansible Deployment') {
            steps {
                script {
                   sleep '360'
                    ansiblePlaybook becomeUser: 'ec2-user', credentialsId: 'amazonlinux', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansible-tf/ansible-task/inventory.yaml', playbook: '/var/lib/jenkins/workspace/ansible-tf/ansible-task/amazon-playbook.yml', vaultTmpPath: ''
                    ansiblePlaybook become: 'ubuntu', credentialsId: 'amazonlinux', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/ansible-tf/ansible-task/inventory.yaml', playbook: '/var/lib/jenkins/workspace/ansible-tf/ansible-task/ubuntu-playbook.yml', vaultTmpPath: ''
                }
            }
        }
    }
}
