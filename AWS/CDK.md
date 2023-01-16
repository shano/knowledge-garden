
* Supports all the resources the [[Cloudformation]] supports
* Use the [reference guide](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-construct-library.html). [Python Reference](https://docs.aws.amazon.com/cdk/api/latest/python/index.html)
	* Any props with a ? is optional.
* Python
	* Setup
		* `from aws_cdk import (aws_s3 as s3)` - Import the package
		* `pip install aws-cdk.aws-s3` Install the package
		* `cdk initla
		* `source .env/bin/activate`
		* `pip install -r requirements.txt`
	* `cdk initla
* CLI
	* Boilerplate
		* `cdk init` to set things up
		* `virtualenv`
		* `cdk bootstrap` - Sets up all the backend s3 stuff to run deployments
	* `cdk diff` - Show what changes
	* `cdk synth` - Show Cloudformation
	* `cdk deploy` - Deploy the thing
* EC2
	* `LookupMachineImage` is a handy way to get an AMI to drive EC2.
	* Try populate things like `owner` etc as it'll speed things up.
	* `VPC.fromLookup()` - gets the lookup
	* Specify env specifically
* Cleanup
	* Comment out the whole stack and run `cdk deploy` and things will be destroyed or orphaned
