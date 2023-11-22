#pipeline {
#    agent  { label 'node' } 
#
#    parameters {
#        choice(name: 'TERRAFORM_ACTION', choices: ['init', 'apply', 'destroy'], description: 'Select Terraform Action')
#    }
#
#    stages {
#        stage('Terraform Init, Apply, or Destroy') {
#            steps {
#                script {
#                    switch (params.TERRAFORM_ACTION ) {
#                       case 'init':
#                           echo "Running Terraform Init"
#                           sh 'terraform init'
#                           break                       
#                        case 'apply':   
#                            echo "Running Terraform Apply"
#                            sh 'terraform apply --auto-approve'
#                            break
#                        case 'destroy':
#                            echo "Running Terraform Destroy"
#                            sh 'terraform destroy --auto-approve'
#                            break
#                        default:    
#                            error "Invalid Terraform action selected"
#                    }
#                }
#            }
#        }
#    }
#
#    post {
#        always {
#            echo 'Execution completed'
#        }
#        success {
#            echo 'Successfully executed Terraform action'
#        }
#        failure {
#            echo 'Terraform action failed'
#        }
#    }
#}
#

pipeline {
    agent  { label 'node' } 

    stages {
        stage('Install Ansible') {
            steps {
                script {
                    def ansibleInstalled = sh(script: 'which ansible', returnStatus: true)
                    if (ansibleInstalled != 0) {
                        echo "Ansible is not installed. Installing now..."
                        sh '''
                            sudo apt-add-repository -y ppa:ansible/ansible
                            sudo apt update
                            sudo apt install -y ansible
                        '''
                    } else {
                        echo "Ansible is already installed. Skipping installation."
                    }
                }
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                script {
                    sshagent(['jenkins-user-ssh-key']) {
                        sh '''#!/bin/bash
                        ansible all -i inventory.ini -m ping
                        ansible-playbook -i inventory.ini playbook.yml 
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            sh 'echo "This will always run"'
        }
        success {
            sh 'echo "Build Was Successfull"'
        }
        failure {
            sh 'echo "Build Failed"'
        }
    }
}        