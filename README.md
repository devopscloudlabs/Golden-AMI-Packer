# Creating a Golden AMI with Packer

## Step 1: Pipeline Configuration 

This section defines a Jenkins pipeline (or any CI/CD tool) to automate the AMI building process. It retrieves AWS credentials from a secret and executes Packer commands in separate stages.

## Step 2: Packer Configuration

Required Plugins: Defines the amazon plugin with version and source for interacting with AWS services.
Source AMI: Configures the amazon-ebs builder to:
  Use ap-southeast-2 region.
  Create an AMI named ami-version-1.0.1-{{timestamp}}.
  Use micro instance type t2.micro.
  Specify a base AMI ID ami-0d6294dcaac5546e4.
  Set ec2-user for SSH access.
  Limit deployment region to ap-southeast-2.
Build Configuration: Defines the hq-packer build:
  References the source.amazon-ebs.amazon-linux source.
  Uses a series of shell provisioners to:
  Copy provisioner.sh script to the instance.
  Make the script executable.
  List the contents of /tmp directory (for debugging).
  Print the working directory.
  Print the content of provisioner.sh.
  Execute the provisioner.sh script with /bin/bash -x for verbose output.
  
## Step 3: Provisioning Script (provisioner.sh)

This script defines the software installations and configurations to be applied to the AMI:

Update Packages: Updates the package list using sudo yum -y update.
Install Git: Installs git using sudo yum install git -y.
Install SSM Agent: Downloads and installs the AWS Systems Manager Agent (amazon-ssm-agent) for remote management. While the script installs it, starting the service is commented out (#sudo systemctl start amazon-ssm-agent).
Install CloudWatch Agent: Downloads and installs the Amazon CloudWatch Agent (amazon-cloudwatch-agent) for monitoring. The script starts (sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a start) and checks the status (sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status) of the agent.
Install AWS Inspector: Downloads and installs the AWS Inspector agent for vulnerability scanning.
Install Docker: Installs Docker (sudo yum install docker -y) and starts the service (sudo systemctl start docker).
