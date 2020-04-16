pipeline {
    agent any
    parameters {
        string(name: 'project_name', defaultValue: 'Packer Pipeline', description: 'Jenkins Pipeline for terraform?')
    }
    environment {
        terraform_version = '0.11.11'
        packer_version = '1.4.3'
        access_key = 'input_your_access_key'
        secret_key = 'input_your_secret_key'
    }
    stages {
         // stage('Install Terraform') {
           //   steps {
                    //sh "sudo apt-get install wget zip -y"
                    //sh "cd /tmp"
                    //sh "curl -o terraform.zip https://releases.hashicorp.com/terraform/0.12.24/terraform_0.12.24_linux_amd64.zip"
                    //sh "ls -l; pwd; unzip -o terraform.zip"
                    //sh "sudo mv ./terraform /usr/bin"
                    //sh "sudo rm -rf bin_terraform.zip"
                    //sh "terraform version"
             // }
         // }
          stage('Install Packer') {
              steps {
                   // sh "sudo apt-get install wget zip -y"
                    sh "cd /tmp"
                    sh "curl -o /tmp/bin_packer.zip https://releases.hashicorp.com/packer/$packer_version/packer_'$packer_version'_linux_amd64.zip"
                    sh 'pwd'
                    sh "cd /tmp && unzip -o /tmp/bin_packer.zip"
                    sh "sudo mv /tmp/packer /usr/bin"
                    sh "sudo rm -rf /tmp/bin_packer.zip"
                    sh "packer version"
              }
          }
          stage('code checkout') {
               steps {
                    git branch: 'master', url: 'https://github.com/markwu100/packer.git'
                    }
          }
          stage('Build AMI') {
                steps {
                    dir('./packer'){
                     sh 'ls -la; pwd; packer build template.json'
                    }
                }
          }
          stage('Deploy??') {
                steps {
                    script {
                       timeout(time: 2, unit: 'MINUTES') {
                          input(id: "Deploy Gate", message: "Want to Deploy ${params.project_name}?", ok: 'Deploy??')
                       }
                    }
                }
          }
         stage('Terraform Deploy'){
             steps {
                 dir('./terraform'){

                 sh  """
                     terraform init; terraform plan; terraform apply -auto-approve -var 'access_key=$access_key' -var 'secret_key=$secret_key'
                     """

                 }

             }
         }
    }
}
