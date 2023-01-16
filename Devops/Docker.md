# Docker

Docker is an application build and deployment tool use heavily at many organisations.


* Tools
	* [[ECS]]
	* [[ECR]]
	* [[Swarm]]
* Good one-liners
	* `docker ps -lq | xargs echo "Heeey Buuuuddy its a container ID  --> " `
		* Run docker and get container id
	* `docker run -i -t --name mittens debian /bin/bash`
		* Run a docker container with a name
	* `docker build --tag project/version .`
		* Build container with a tag
	* `docker run -I -t --name myservice some/name --publish 8080:8080`
		* Run specific tag and publish on certain port
	* `docker logs -f <container_id>`
		* Watch logs for a particular container

