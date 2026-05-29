pipeline {
    agent any

    environment {
        TF_PLUGIN_CACHE_DIR = '/var/lib/jenkins/.terraform.d/plugin-cache'

        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION    = 'us-west-2'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/SUMITBBS/3-tier-architecture.git'
            }
        }

        stage('Terraform Init') {
            steps {
                sh '''
                    rm -rf .terraform
                    terraform init -no-color
                   '''
            }
        }

        stage('Terraform Format') {
            steps {
                sh 'terraform fmt -check'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate -no-color'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -no-color'
            }
        }

        stage('Terraform Apply') {
            steps {
                input message: 'Do you want to apply Terraform changes?'
                sh 'terraform apply -auto-approve'
            }
        }

        stage('Terraform Destroy') {
 	   steps {
       		 input message: 'Approve Terraform Destroy?'
       		 sh 'terraform destroy -auto-approve -no-color'
    		
	   }
	}	
    }
    
    post {

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Please check console output.'
        }
    }
}
