# Terraform Multi-Region EC2 Deployment

This Terraform configuration launches Linux EC2 instances in two AWS regions (`us-east-1` and `us-west-2`) using a single terraform configuration file.
Screenshots of my execution are attached for reference.

---

## **Prerequisites**
1. Terraform CLI : https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli.
2. AWS CLI installed: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html.
3. Access and secret key for AWS account

## **Steps :**
- aws configure with access key and secret key
- Create a configuration file with .tf extension, The below syntax will create two EC2 instance in us-east-1 and us-west-1 and using output block to display the public IP's of the instances. Verify the AMI exists in the specified region
  ```bash
   provider "aws" {
    region = "us-east-1"
    alias = "east"
  }
  provider "aws" {
    region = "us-west-1"
    alias = "west"
  }
  resource "aws_instance" "sample-east" {
    provider = aws.east
    ami = "ami-04b4f1a9cf54c11d0"
    instance_type = "t2.micro"
    tags = {
        Name = "sample-instance-east"
    }
  }
  resource "aws_instance" "sample-west" {
    provider = aws.west
    ami = "ami-07d2649d67dbe8900"
    instance_type = "t2.micro"
    tags = {
        Name = "sample-instance-west"
    }
  }
  output "aws_instance_sample-east" {
    value = aws_instance.sample-east.public_ip
  }
  output "aws_instance_sample-west" {
    value = aws_instance.sample-west.public_ip
  }

- Optional: We can use variables in the configuration file to provide ami_id, instance_type... I choose to keep this plain.
- On the present directory, use init command to initize terraform where the configuration present
   ```bash
   Terraform init
- Once initilized, Use following commands to terraform plan - validation of changes and terraform apply - executing the changes
  ```bash
  terraform plan
  terraform apply
- After Execution, The EC2 instances will be created in two different regions.

  ## **Conclusion :**
  - To cleanup the resource created, Execute terraform destroy - delete the resources
  ```bash
  terraform destroy 
