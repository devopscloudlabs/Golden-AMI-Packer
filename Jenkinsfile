pipeline 
    agent any 
    stages{
        stage("AWS Demo"){
            steps{
                withCredentials([
                    [
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credebtialsId: 'aws_credential',
                        accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]
                ])  {
                    sh "packer init aws-ami-v1.pkr.hcl"
                    sh "pakcer build aws-ami-v1.pkr.hcl"
                }
            }
        }
    }