
### Cloudformation Designer
* Useful for validating cloudformation
#### Create Template in Designer
* Looks like a good way to build up a rough template, has neat links between entities
* Can use it along with reference guide to start building up the required fields, which aren't shown in the designer or augment it by copy-pasting template samples in.
* Cmd-Space allows you to autocomplete elements in your template
### StackSets
* Cloudformation is regional, so for Multi-Account/Multi-Region business
* User maintaining a stack across many accounts would need admin access to all.
* *Hub and Spoke Model*
	* Hub is the Shared Services Account
		* Can assume roles in many child accounts
		* Then can deploy stacks in all child accounts
	* Spoke is the Child Account
	* *Roles*
		* Hub needs an IAM role for the spokes
		* So Hub has a role, that give it access to *STS Assume Role*
			* This STS Assume role lets them take the role of child accounts.
			* Documentation for setting up roles [here](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/stacksets-prereqs-self-managed.html)
* Basically once setup you can create Stacksets on the admin(or hub) and this will act on the child accounts.
	* You can pass in AccountIds to submit to those children.
### Stacks
* Collection of resources
* Use the stacks on CloudFormation to get a good status of deploys.
	* Events
	* Resources
		* Custom Resources
			* Eg. Call Lambda to get the latest AMI ID to always use an up to date EC2 Instance.
	* Outputs
* *Nested Stacks*
	* Best Practice
	* Nested Stack is stored in an S3 bucket
		* Called using S3 url
		* Example: S3 bucket with output
```
AWSTemplateFormatVersion: '2010-09-09'
Resources:
S3Bucket:
Type: AWS==S3==Bucket
Outputs:
BucketName:
Value: !Ref 'S3Bucket'
Description: Name of the sample Amazon S3 bucket.`- Then using the BucketName in the calling stack -`AWSTemplateFormatVersion: 2010-09-09
Resources:
NestedCall:
Type: AWS==CloudFormation==Stack
Properties:
TemplateURL: https://testbucket.s3-us-west-2.amazonaws.com/s3cft.yml
TimeoutInMinutes: 60
Outputs:
StackRef:
Value: !Ref NestedCall
OutputFromNestedStack:
Value: !GetAtt NestedCall.Outputs.BucketName`- Or for sending parameters to the nested stack -`AWSTemplateFormatVersion: 2010-09-09
Parameters:
Emailaddress:
Type: String
Description: 'Enter email for SNS notification'
Resources:
NestedCall:
Type: AWS==CloudFormation==Stack
Properties:
Parameters:
SNSEmail: !Ref Emailaddress
TemplateURL: https://testbucket.s3-us-west-2.amazonaws.com/sns_parameter_example.json
TimeoutInMinutes: 60
Outputs:
StackRef:
Value: !Ref NestedCall
```


### Changeset
	l	- Proposed changes to a stack
- Awesome for reviewing changes before applying the change.
	* You can also review the creation of the template by creating a changeset, not the stack.
### Tips for building templates
	* Use an existing template
	* Just google for sample templates for the particular service you want here or find [here](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/sample-templates-services-us-west-2.html)
* [Cloudformation Templates](Cloudformation Templates)
	* AWS Console
		* You can import the file directly
		* The based on parameters set you can specify specific parameters
		* Update stack
			* You can modify the json and upload
			* You can also update parameters
	* *Via CLI*
		* On the docs if a parameter isn't surrounded with [] it's required.
		* Create a stack
			* `aws cloudformation create-stack --stack-name docker-on-aws --template-body file://ecs-cluster.json --parameters ParameterKey=Keyname,ParameterValue=somekey`
		* Watch stack events
			* `watch -n 15 was cloudformation describe-stack-events --stack-name docker-on-aws --max-items 5`
		* Delete the stack
			* `aws cloudformation delete-stack --stack-name docker-on-aws`
		* Update stack
			* `aws cloudformation update-stack --stack-name docker-on-aws --use-previous-template --parameters ParameterKey=KeyName,UsePreviousValue=true ParameterKey=ClusterSize,ParameterValue=1`
			* Note the use-previous-template and UsePreviousValue settings
			* This is updating one of the clusters parameters
	* *Cost Estimation*
		* Under Create Stack Review Screen, there is an estimated cost function.
	* *Helper Scripts*
		* [[Python]] scripts to help you install software/start services on EC2 Instances created in Cloudformation.
		* User Data is very limited in the scope of cloudformation eg. getting output from the scripts etc.
		* Not really used very much, better alternatives like SSM/Ansible
	* *Cloudformer*
		* Creates cloudformation from your existing ec2 infrastructure
		* It's a toy right now, in beta and should *not be used for production*.
	* *Tips*
		* You can easily on AWS convert YAML to JSON under Cloudformation console
		* You can submit "EC2 User Data" from Cloudformation to an EC2 Instance.
			* Look at the EC2 Template for spinning up an ec2 instance
			* In the template file Resources->EC2Instance->Properties
			* 
		* ```UserData:
```Fn::Base64: !Sub |
＃!/bin/bash
yum install httpd -y
service httpd start
echo "Hello from Region ${AWS::Region}" > /var/www/html/index.htm
```

* [[IAM]] *with Cloudformation*
	* If you don't provide/attach a role, cloudformation will assume the role of the user.
	* Always utlise an attached role for cloudformation.
* *Drift Detection*
	* View changes done directly rather than using Cloudformation
	* In stack actions detect then view drift results.
* *Best Practices*
	* Reuse templates to replicate stacks in multiple environments
		* Use Parameters/Mappings/Conditions with the same template for dev/test/prod
	* Use Nested Stack to Reuse Common Template Patterns
	* Never embed credentials in your template
		* Input Parameters with `NoEcho: true` so they're hidden
		* Or use Dyanmic Params with Systems Manager/Secrets Manager.
	* Use AWS-Specific Parameter Type
		* Reduces user error and gives drop-downs with valid values
		* Use Parameter Constraints
			* Mix/Max length, regex checks etc
	* Validate Template before using them
		* `aws cloudformation validate-template`
	* Don't let the stack drift
	* Use Change Sets
	* Use Code Reviews and Revision Control for Cft

#### Cloudformation with [Secrets Manager](Secrets Manager)
	* Example template where we generate a password the setup a DB with that password
```
AWSTemplateFormatVersion: 2010-09-09
Description: >-
AWS CloudFormation Template tyo demonstrate creating a secret
and using it in a RDS instance
Resources:
MyRDSSecrets:
Type: AWS==SecretsManager==Secret
Properties:
Description: 'This is the secret for my RDS instance'
GenerateSecretString:
SecretStringTemplate: '{"username": "admin"}'
GenerateStringKey: 'password'
PasswordLength: 16
ExcludeCharacters: '"@/'
MyDBInstance:
Type: AWS==RDS==DBInstance
Properties:
AllocatedStorage: 20
DBInstanceClass: db.t2.micro
Engine: mysql
MasterUsername: !Join ['', ['{{resolve:secretsmanager:', !Ref MyRDSSecrets, ':SecretString:username}}' ]]
MasterUserPassword: !Join ['', ['{{resolve:secretsmanager:', !Ref MyRDSSecrets, ':SecretString:password}}' ]]
BackupRetentionPeriod: 0
DBInstanceIdentifier: 'rds-instance'
```

#### Cloudformation with [Parameter Store](Parameter Store)
```
{
"AWSTemplateFormatVersion": "2010-09-09",
"Resources" : {
"MyS3Bucket": {
"Type": "AWS==S3==Bucket",
"Properties": {
"AccessControl": "{{resolve:ssm:S3AccessControl:1}}"
}
}
}
}  ```
* *Safeguarding Cloudformation*
	* Termination Protection
		* Prompts disabling
		* Set IAM role to ensure only specific users can change termination protection
	* Deletion Policy
		* `DeletionPolicy: Retain`
	* IAM Policy
		* `"action": "cloudformation:DeleteStack"`
	* Stack Policy
		* Applied to one stack only
		* Under console, Advanced Option in Cloudformation
		* ```{
```"Statement" : [
{
"Effect" : "Allow",
"Action" : "Update:/",
"Principal": "/",
"Resource" : "/"
},
{
"Effect" : "Deny",
"Action" : "Update:/",
"Principal": "*",
"Resource" : "LogicalResourceId/ProductionDatabase"
}
]
}
```

- This ONLY applies to updates, so update:delete etc, an actual deletion of the stack relies on the IAM role, so use termination protection instead.

## CFN-Hup
#blog #til #tech/aws
CFN-Hup is a cloudformation daemon that detects and applies changes to the EC2 instance it's running on.