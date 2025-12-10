pipeline {
    agent any

    environment {
        TF_VAR_key_name = 'jenkins'     // 🔥 Terraform will use AWS key-pair "jenkins"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Terraform Init & Apply') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-creds']]) {
                    sh 'terraform init -input=false'
                    sh 'terraform validate'
                    sh 'terraform plan -out=tfplan -input=false'
                    sh 'terraform apply -input=false -auto-approve'
                }
            }
        }

        stage('Wait for instances') {
            steps {
                sh 'sleep 30'
            }
        }

        stage('Run Ansible - Frontend') {
            steps {
                ansiblePlaybook(
                    credentialsId: 'jenkins-key',    // 🔥 SSH key matches jenkins.pem
                    disableHostKeyChecking: true,
                    installation: 'ansible',
                    inventory: 'inventory.yaml',
                    playbook: 'amazon-playbook.yml',
                    become: true,
                    extraVars: [
                        ansible_user: "ec2-user"     // Amazon Linux user
                    ]
                )
            }
        }

        stage('Run Ansible - Backend') {
