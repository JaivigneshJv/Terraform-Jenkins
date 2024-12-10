pipeline {
    agent { label 'terraform-agent' }

    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-access-key')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-key')
        AWS_SESSION_TOKEN = credentials('aws-session-token')
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code...'
                checkout scm
            }
        }

        stage('Terraform Init') {
            steps {
                container('terraform') {
                    echo "Running Terraform Init..."
                    sh "terraform init"
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                container('terraform') {
                    echo "Running Terraform Plan..."
                    sh "terraform plan"
                }
            }
        }

        stage('Approval') {
            steps {
                input "Approve to Apply Terraform Changes?", ok: "Approve"
            }
        }

        stage('Terraform Apply') {
            steps {
                container('terraform') {
                    echo "Running Terraform Apply..."
                    sh "terraform apply -auto-approve"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline execution completed!"
        }
    }
}

