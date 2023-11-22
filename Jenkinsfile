pipeline {
    agent { slave-node { label 'node' } }

    parameters {
        choice(name: 'TERRAFORM_ACTION', choices: ['init-apply', 'destroy'], description: 'Select Terraform Action')
    }

    stages {
        stage('Terraform Init and Apply or Destroy') {
            steps {
                script {
                    if (params.TERRAFORM_ACTION == 'init-apply') {
                        echo "Running Terraform Init and Apply"
                        sh '''
                           terraform init
                           terraform apply --auto-approve
                        '''
                    } else if (params.TERRAFORM_ACTION == 'destroy') {
                        echo "Running Terraform Destroy"
                        sh '''
                           terraform destroy --auto-approve
                        '''
                    } else {
                        error "Invalid Terraform action selected"
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Execution completed'
        }
        success {
            echo 'Successfully executed Terraform action'
        }
        failure {
            echo 'Terraform action failed'
        }
    }
}
