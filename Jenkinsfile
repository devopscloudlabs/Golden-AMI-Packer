pipeline {
    agent any
    stages{
        stage('Building Resources') {
          steps {
              sh 'apt-get update'
              sh 'apt-get install packer'
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
