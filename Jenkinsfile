def git_url = 'https://github.com/linuxag/terraform-aws'
def git_branch = 'main'

pipeline
{
    agent {
        label 'worker1'
    }
    options{
        timestamps()
        timeout(time: 30, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }
    parameters {
                booleanParam(name: 'create_resources',defaultValue: true, description: 'Create resources' )
                booleanParam(name: 'destroy_resources',defaultValue: false, description: 'Destroy resources' )
    }
    stages
    {
        /*stage('Git-checkout')
        {
            steps
            {
                git credentialsId: 'github', url: git_url , branch: git_branch
            }
        }*/
        stage('terraform-init')
        {
            steps
            {
                sh '''
               
                terraform init
                '''
            }
        }
        stage('terraform-validate')
        {
            steps
            {
                sh '''
                
                #terraform validate
                '''
            }
        }
        stage('terraform-plan')
        {
            steps
            {
                sh '''
                
                terraform plan
                '''
            }
        }
        stage('terraform-approval')
        {
            steps
            {
                script{
                    def userInput = input(id: 'confirm', message: 'apply terraform?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'apply terraform', name: 'confirm'] ])
                }
            }
        }
        stage('terraform-apply')
        {
            when { 
                    expression 
                    { 
                        params.create_resources == true 
                    } 
            }
            steps
            {
                sh '''
                
                terraform apply -auto-approve
                '''
            }
        }
        stage('terraform-destroy')
        {
            when { 
                    expression 
                    { 
                        params.destroy_resources == true 
                    } 
            }
            steps
            {
                sh '''
                cd terraform
                terraform destroy -auto-approve
                '''
            }
        }
    }
}