
* Trusted Entity
	* A role you can attach to internal services to be trusted(eg. [CodePipeline](CodePipeline.md))
* Trust Policies
	* A Policy you can use to define permission for resources
	* Associated with an identity or service
	* For Services
		* Attached to the AWS Service Role
		* Adding an IAM role to a running service(like ec2), automatically give it it's credentials
	* For Users
		* Add to a group and add users to that group
* Role Based Access