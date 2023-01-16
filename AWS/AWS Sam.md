
* Extends [[Cloudformation]] for Serverless Application Model
### Why

* Shorthand Syntax for lambdas, api, database, event source mappings etc
* Lots of abstract around cloudformation
* Can test and debug locally
* Good integration with dev tools, ides, jenkins etc.

###  Documentation
* Good docs on their [Github Profile](https://github.com/awslabs/serverless-application-model)

### Deploy Lambda with SAM

```
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Description: Test Pipeline Lambda
Resources:
hellofunction:
Type: 'AWS==Serverless==Function'
Properties:
Handler: hello_country.lambda_handler
Runtime: python3.7
CodeUri: ./lambdacode/hello_country.py
Description: Sample SAM Lambda Deployment
Events:
S3Event:
Type: S3
Properties:
Bucket: !Ref S3bucket
Events: s3:ObjectCreated:*
S3bucket:
Type: AWS==S3==Bucket            
```

- This will generate the cloudformation compatible code and put in an s3 bucket
	- `sam package --template-file lambda-sam.yml --s3-bucket demo-test-101 --output-template-file packaged-lambda.yml`
#### To deploy
- This produces a super dumb lambda, that just has access to IAM
	- `sam deploy --template-file lambda-sam.yml --stack-name lambdastack --capabilities CAPABILITIES_IAM`
- Setup Lambda with dependencies
	- `sam build --template lambda-dependency-sam.yml --manifest ./requirements.txt`
- Then sam package will grab all the dependencies
#### Testing locally
	- `sam local invoke --event ./lambdacode/test.json`
- test.json contains a sample event arguments like so`{"Country": "India"}`
#### Mixing in with Cloudformation
	* You can embed Sam in the same file.
	* But not all cloudformation functionality is support on the Sam side.
	* Supports parameters, mappings, outputs etc along with intrinsic functions
	* Need this in the template `  "Transform": "AWS::Serverless-2016-10-31",`
#### Event Source Types see [here](https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#event-source-types)
* S3 Bucket Events
* API
	* See example folder
* Scheduled Lambda