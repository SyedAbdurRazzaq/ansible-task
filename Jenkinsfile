pipeline {
    agent any

    stages {
        

        stage('Checkout') {
            steps {
                sh 'echo cloning repo'
                git branch: 'main' , url: 'https://github.com/saivarun0509/ansible-task.git' 
            }
        }
        
        stage('Terraform Apply') {
            steps {
                script {
                    dir('ansible-task/main.tf') {
                    sh 'terraform init'
                    sh 'terraform validate'
                    sh 'terraform plan'
                    }
                }
            }
        }
        
        stage('Ansible Deployment') {
            steps {
                script {
                   sleep '60'
                    ansiblePlaybook becomeUser: 'ec2-user', credentialsId: 'amazonlinux', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/challenge' , playbook: '/var/lib/jenkins/workspace/challenge', vaultTmpPath: ''
                    ansiblePlaybook become: true, credentialsId: 'ubuntuuser', disableHostKeyChecking: true, installation: 'ansible', inventory: '/var/lib/jenkins/workspace/challenge', playbook: '/var/lib/jenkins/workspace/challenge', vaultTmpPath: ''
                }
            }
        }
    }
}
