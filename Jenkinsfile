pipeline {
    agent any
    stages {
        stage('Preparing') {
            steps {
                echo 'Preparing the environment'
            }
        }
        stage('Git Pulling') {
            steps {
                git branch: 'master', url: 'https://github.com/Bhaktabahadurthapa/EKS-Terraform-GitHub-Actions.git'
            }
        }
        stage('Init') {
            steps {
                withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                    script {
                        try {
                            echo 'Initializing Terraform...'
                            sh 'terraform -chdir=eks/ init -reconfigure'
                        } catch (Exception e) {
                            error "Terraform Init failed: ${e.message}"
                        }
                    }
                }
            }
        }
        stage('Validate') {
            steps {
                withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                    script {
                        try {
                            echo 'Validating Terraform configuration...'
                            sh 'terraform -chdir=eks/ validate'
                        } catch (Exception e) {
                            error "Terraform Validate failed: ${e.message}"
                        }
                    }
                }
            }
        }
        stage('Action') {
            steps {
                withAWS(credentials: 'aws-creds', region: 'us-east-1') {
                    script {    
                        try {
                            echo "Executing Terraform action: ${params.Terraform_Action}"
                            if (params.Terraform_Action == 'plan') {
                                sh "terraform -chdir=eks/ plan -var-file=${params.Environment}.tfvars"
                            } else if (params.Terraform_Action == 'apply') {
                                sh "terraform -chdir=eks/ apply -var-file=${params.Environment}.tfvars -auto-approve"
                            } else if (params.Terraform_Action == 'destroy') {
                                sh "terraform -chdir=eks/ destroy -var-file=${params.Environment}.tfvars -auto-approve"
                            } else {
                                error "Invalid value for Terraform_Action: ${params.Terraform_Action}"
                            }
                        } catch (Exception e) {
                            error "Terraform Action failed: ${e.message}"
                        }
                    }
                }
            }
        }
    }
}
