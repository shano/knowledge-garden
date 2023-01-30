# Terraform
* What is it?
	* Infrastructure as code, like AWS cdk.
* Hashicorp Configuration Language (HCL)
	* Expressions
	* Blocks
	* Arguments
* Providers
	* ```provider "aws" {
```profile    = "default"
region     = "us-east-1"
}```
* Resources
	* ```resource "aws_instance" "example" {
```ami           = "ami-2757f631"
instance_type = "t2.micro"
}```
* Hooking into AWS
	* You can use a limited IAM user instead of using secret/access key.
* CLI
	* `terraform init` - Setups things up
	* `terraform apply` Runs the .tf file and asks to appl
* Tip
	* Use the docs heavily to see example usage