
* Can handle all the steps to deploy/test code from a commit
* Can integrate with a bunch of 3rd party tools
* Benefits
	* Rapid Delivery
	* Configurable Workflow
	* Fully Manage
* Source
	* Where do you want to get the code(ECR, Github, etc)
* Build
	* Jenkins or Codebuild etc
* Deploy
	* Where do you want to deploy to (EBS, CodeDeploy etc)
* You can also add custom stages
* Tips
	* Don't create New Service Roles within codepipeline
	* It makes very restrictive roles
* [[Cloudformation]]
	* Integrations
		* Create/Update a stack
		* Use a changeset
			* Step 1 - Deploy initially just created a changeset (rename to create change set)
			* Step 2a - Add a new action group after the to run after creating change set,
			* Step 2b - Action mode is execute a changeset
	* Passing Parameters
		* You can pass parameters from files in github
		* Template Configuration file that sits in the repo can be pointed to
		* See example [here](https://github.com/saha-rajdeep/cloudformation-demo/blob/master/sns-template-parameter.json)
	* The Artifact is the filename in the source repo for [[Cloudformation]]