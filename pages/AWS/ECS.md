
* Elastic Container services for [[AWS]]
* Cluster
	* Group of containers that run specific tasks
* Containers
	* docker
* Service
	* Binds a task definition to a cluster by a running task
* Task Definitions
	* Things to be run on an ecs cluster
* Linking Tasks
	* Within the same task definition
		* In the containers in the task definition
			* You are able to add links between tasks
	* Within different task definitions(simple option)
		* Create an ELB for the thing you want to link to
		* Then in env variables pass in the ELB dns
	* Within different task definitions(complex)
		* Use service discovery
* Logs
	* AWS Way
		* Under cloudtrail, configure buckets etc first
	* To a log Agreegator
		* Use logspout container as an aggregator
		* Add a container with say logspout
			* Create a mount with source /var/run/docker.sock
		* Then in the mount point
			* Src is the volume we just added then into /tmp/docker.socket
* CPU Units
	* 1024 is a micro cpu
* Memory
* Running via command line
	* You can export the json from a task definition as a good starting point
* Cli Apps
	* aws ecs register-task-definition --cli-input-json file:slashslashfilename.json
		* Setup a task definition based on that input json
	* aws ecs list-clusters
		* Get all the clusters
	* aws ecs list-task-definitions
		* List all task definitions
	* aws ecs create-services --cluster name --task-definition service_name:1 --desired-count 1 --service-name service_name
		* Setup a task running on the cluster "name" based on service_name task definition
	* aws ecs describe-services --cluster name --service service_name
* Local development options
	* docker-compose
* Further Research
	* Consul as service discovery(or use Amazon's)