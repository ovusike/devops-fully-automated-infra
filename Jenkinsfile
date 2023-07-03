def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]

pipeline {
    agent any

    tools {
        terraform 'Terraform'
    }



    
    stages {
        stage('Git checkout') {
            steps {
                echo 'Cloning project codebase...'
                git branch: 'main', url: 'https://github.com/ovusike/devops-fully-automated-infra.git/'
                sh 'ls'
            }
        }
        
         stage('Verify Terraform Version') {
            steps {
                echo 'verifying the terrform version...'
                sh 'terraform --version'
               
            }
        }
        
        stage('Terraform init') {
            steps {
                echo 'Initiliazing terraform project...'
                sh 'terraform init'
               
            }
        }
        
        
        stage('Terraform validate') {
            steps {
                echo 'Code syntax checking...'
                sh 'terraform validate'
               
            }
        }
        
        
        stage('Terraform plan') {
            steps {
                echo 'Terraform plan for the dry run...'
                sh 'terraform plan'
               
            }
        }

        stage('Checkov scan') {
            steps {
                sh """
                pip3 install checkov --use-feature=2020-resolver
                checkov -d .
                """
               
            }
        }
        
        stage('Manual approval') {
            steps {
                
                input 'Approval required for deployment'
               
            }
        }
        
        
         stage('Terraform apply') {
            steps {
                echo 'Terraform apply...'
                sh 'terraform apply --auto-approve'
               
               
            }
        }

        
        stage('Manual destroy approval') {
            steps {
                
                input 'Approval required to destroy'
               
            }
        }

        stage('Terraform destroy') {
            steps {
                echo 'Terraform destroy...'
                sh 'terraform destroy --auto-approve'
        
        
    }
    }
    }

    post { 
            always { 
                echo 'I will always say Hello again!'
                slackSend channel: '#integrating-slack-to-jenkins', color: COLOR_MAP[currentBuild.currentResult], message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
            }
        }
}
