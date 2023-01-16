# Cloudformation Templates
* Declarative JSON description of params, resources, outputs
* [Cloudformation Parameters](Cloudformation Parameters) - Move to own page
	* Max - 60
	* Type - String etc, AWS-Specific or SSM Parameter Type
	* AWS Specific Parameter Types
		* ```"KeyName": {
```"Description": "Name of an existing EC2 KeyPair to enable SSH access to the instance",
"Type": "AWS==EC2==KeyPair::KeyName",
"ConstraintDescription": "must be the name of an existing EC2 KeyPair."
},```
	* Use default value as good practice
	* Can do lots of checks min/max or AllowedPattern regex etc
	* Inputs to your system - Used a LOT
	* Config/Other cloudstacks etc
	* Eg. ECS Cluster size
	* ```"Parameters" : {
```"InstanceTypeParameter" : {
"Type" : "String",
"Default" : "t2.micro",
"AllowedValues" : ["t2.micro", "m1.small", "m1.large"],
"Description" : "Enter t2.micro, m1.small, or m1.large. Default is t2.micro."
},
},```
	* And to reference:
	* ```"Resources" : {
```"myEC2Instance" : {
"Type" : "AWS==EC2==Instance",
"Properties" : {
"IamInstanceProfile" : {"Ref" : "IAMRoleparameter"},
"ImageId" : { "Ref" : "AMIIdParameter"},
"InstanceType" : { "Ref" : "InstanceTypeParameter" }
}
}
}```
	* Pseudo Parameters
		* AWS==Region, AWS==AccountId etc for getting various bits of data
* Mappings
	* They allow you to provide conditional logic in the templates
	* Eg. Which ami to use for which region -> lookup trees
	* Can also be used w/ input paramaters too
	* *Defining*
		* ```"Mappings": {
```"AWSInstanceType2Arch": {
"t1.micro": {
"Arch": "HVM64"
},
"t2.nano": {
"Arch": "HVM64"
},
"t2.micro": {
"Arch": "HVM64"
},```
	* *Calling*
		* ```"Fn::FindInMap": [
```"AWSInstanceType2Arch", //Map name
{
"Ref": "InstanceType" //The Top Level Key
},
"Arch" // The Second Level Key
]```
* Resources
	* Meat of the template - THESE ARE REQUIRED
	* Defining
		* ```"Resources" : {
```"myEC2Instance" : {
"Type" : "AWS==EC2==Instance",
"Properties" : {
"IamInstanceProfile" : {"Ref" : "IAMRoleparameter"},
"ImageId" : { "Ref" : "AMIIdParameter"},
"InstanceType" : { "Ref" : "InstanceTypeParameter" }
}
}
}```
	* Review the [template docs](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html) for what resources are required. *They are awesome*.
	* Eg. ELB Security Group
* Outputs
	* Provides useful outputs for your stack
	* Eg. Url which calls a function to get the DNS name of a service
	* You can see the output in console, under cloudformation, handy when you quickly want the url of the thing you've made.
	* Search for resources like aws==ec2==instance to see what GetAtt can return for that resource.
	* Defining
		* ```"Outputs": {
```"AZ": {
"Description": "Availability Zone of the newly created EC2 instance",
"Value": {
"Fn::GetAtt": [
"EC2Instance",
"AvailabilityZone"
]
}
},
}```
	* Cross-Stack Outputs
		* Eg. So two teams share outputs from one teams stack to another teams stack
		* Eg. Security Teams handles/creates security groups and app teams use those.
		* Define in one stack like so(note a custom name is used):
			* ```Outputs:
```EC2SecurityGroup:
Description: Security Group to be used in EC2
Value: !GetAtt
- InstanceSecurityGroup
- GroupId
Export:
Name: !Sub '${AWS::StackName}-SecurityGroupId'`- Use in a Stack like so -`Resources:
EC2Instance:
Type: 'AWS==EC2==Instance'
Properties:
InstanceType: !Ref InstanceType
SecurityGroupIds:
- !ImportValue
'Fn::Sub': '${SecurityGroupStackName}-SecurityGroupId```
* Conditions
	* Defining
		* ```"Conditions" : {
```"Determineemail" : {"Fn::Equals" : [{"Ref" : "EnvType"}, "prod"]}
},```
	* Calling
		* `{"Condition":"Determineemail"}`
* Fns/Refs
	* Useful for referencing another resource/function
	* Or to call a particular function -> see the intrinsic function reference
	* Intrinsic Functions
		* Built-in to manage stacks
		* Eg Fn==GetAtt, Ref, Fn==GetAZs, Fn:FindInMap