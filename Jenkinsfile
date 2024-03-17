pipeline {
    agent any
    stages{
        stage('Building Resources') {
          steps {
              // sh ' curl -o packer.zip https://releases.hashicorp.com/packer/1.8.5/packer_1.8.5_linux_amd64.zip'
              // sh 'unzip -f packer.zip'
              // sh 'sudo su'
              // sh 'mv packer /usr/local/bin/'  
              sh 'sudo apt install packer'
          }
        }
        stage("Building AMI") {
            steps {
                withCredentials([
                    [
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'aws_credential',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]
                ]) {
                    sh "packer init ."
                    sh "packer build aws-ami-v1.pkr.hcl"
                }
            }
        }
    }
}
