

## ECS
* Orchestration
* Provides tools to run docker images across any number of machines
### Features
* Auto placement based on resources
* If container fails ECS will recover them by rescheduling them
* Automated load balancing
* Push-button deployment -> Automatically provisions new containers/tear down old images
* Easy cross-instance linking

### ECS: Cluster
* Logical grouping of a system
* Inside are container instances that are just ec2 instances running the ecs agent

### ECS: Task definitions
* Small JSON description of a docker based application 
* Name/Image/Startup commands etc
* ECS Specific terms also
	* Memory/CPU
	* Essential flag
* You then schedule this class within a cluster

### ECS: Scheduling Tasks
* Run Task until completion
* Run as service
* Custom scheduler -> Mesos scheduler


Source: [Deploying Docker Containers to Amazon Web Services | Udemy](https://www.udemy.com/deploying-docker-containers-to-amazon-web-services/)

#tech/docker #tech/aws #tech/aws/ecs #tech/aws/ec2