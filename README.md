https://docs.docker.com/get-started

## Hello World

`docker run hello-world`

## Build new image from DOCKERFILE

`cd /c/Users/dpb6/Downloads/repos/dockertest`
`docker build -t friendlyhello .  # Create image using this directory's Dockerfile`

### Port mapping

`docker-machine ip # Show IP address`

`docker run -p 4000:80 friendlyhello  # Run "friendlyname" mapping port 4000 to 80`
http://192.168.99.100:4000
`docker container ls                                # List all running containers`
`CTRL+C`, `CTRL+C`: This doesn't stop the running container. Need to do so explicitly
`docker container stop <Container NAME or ID hash>           # Gracefully stop the specified container`
`docker container rm <hash>        # Remove specified container from this machine`

`docker run -d -p 4000:80 friendlyhello # Run image as container in detached mode`
`docker container stop <Container NAME or ID hash>           # Gracefully stop the specified container`
`docker container rm <hash>        # Remove specified container from this machine`

## Containers and images

`docker container ls -a             # List all containers, even those not running`
`docker container kill <hash>         # Force shutdown of the specified container`

`docker image ls -a                             # List all images on this machine`

## Tagging and pushing images to DockerHub

`docker login             # Log in this CLI session using your Docker credentials`
(davidbradway)

`docker tag friendlyhello davidbradway/get-started:python3alpine`
`docker tag <image> username/repository:tag  # Tag <image> for upload to registry`

`docker push davidbradway/get-started:python3alpine`
`docker push username/repository:tag            # Upload tagged image to registry`

https://hub.docker.com/r/davidbradway/get-started/

## Pull and run your own image from DockerHub

`docker run -p 4000:80 davidbradway/get-started:python3alpine #Pull and run a DockerHub image`
`docker run username/repository:tag                   # Run image from a registry`

`docker container ls -a             # List all containers, even those not running`

## Start Swarms and Stacks

`docker swarm init --advertise-addr 192.168.99.100 # initialize a new swarm`

`docker stack deploy -c docker-compose.yml getstartedlab # Deploy a 'stack' on this swarm based on a given docker-compose file`
`docker stack deploy -c <composefile> <appname>  # Run the specified Compose file`
`docker stack ls                                            # List stacks or apps`

http://192.168.99.100:8080/

`docker service ls                 # List running services associated with an app`
`docker service ps getstartedlab_web`
`docker service ps <service>                  # List tasks associated with an app`
`docker inspect <task or container>                   # Inspect task or container`
`docker container ls -q                                      # List container IDs`


`docker-machine create --driver virtualbox myvm1 # Create a VM (Mac, Win7, Linux)`
`docker-machine create --driver virtualbox myvm2`
`docker-machine ls # list VMs, asterisk shows which VM this shell is talking to`

NAME      ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
myvm1     -        virtualbox   Running   tcp://192.168.99.100:2376           v18.06.1-ce
myvm2     -        virtualbox   Running   tcp://192.168.99.101:2376           v18.06.1-ce

eval $("C:\Program Files\Docker Toolbox\docker-machine.exe" env myvm1)

`docker-machine ssh myvm1 "mkdir ./data" #Add directory for redis data`

`docker-machine ssh myvm1 "docker swarm init --advertise-addr 192.168.99.100"`

`docker-machine ssh myvm2 "docker swarm join --token SWMTKN-1-36bn1uos57jyjoerh0gm8rxe04pgwakuuakftx3joluy607onc-ahxdckr5kboo49ry6nvdhu1gv 192.168.99.100:2377"`
`# docker-machine ssh myvm2 "docker swarm leave"  # Make the worker leave the swarm`

`docker-machine ssh myvm1 "docker node ls"         # List the nodes in your swarm`
`docker-machine ssh myvm1 "docker node inspect <node ID>"        # Inspect a node`

`docker-machine ssh myvm1   # Open an SSH session with the VM; type "exit" to end`
`docker node ls                # View nodes in swarm (while logged on to manager)`

`docker-machine env myvm1  # View basic node info, env vars, command for myvm1`

`docker stack deploy -c docker-compose.yml getstartedlab # deploy stack to swarm`
`docker stack deploy -c <file> <app>  # Deploy an app; command shell must be set to talk to manager (myvm1), uses local Compose file`

http://192.168.99.101:8080/
http://192.168.99.101:80/

`docker-machine scp docker-compose.yml myvm1:~ # Copy file to node's home dir (only required if you use ssh to connect to manager and deploy the app)`
`docker-machine ssh myvm1 "docker stack deploy -c <file> <app>"   # Deploy an app using ssh (you must have first copied the Compose file to myvm1)`

`docker stack ps getstartedlab # show containers in stack`
`docker service ls`

`docker-machine env myvm1 #Connect shell to mymv1` 
`eval $(docker-machine env myvm1) # Mac command to connect shell to myvm1`
`eval $("C:\Program Files\Docker Toolbox\docker-machine.exe" env myvm1)`

`# eval $(docker-machine env -u) # Disconnect shell from VMs, use native docker`

`# docker-machine stop myvm1 #`
`docker-machine start myvm1   # Start a VM that is currently not running`
`# docker-machine stop myvm2 #`
`docker-machine start myvm2 # Start a VM that is currently not running`
`# docker-machine ssh myvm1 "docker swarm leave -f" # Make master leave, kill swarm`

`docker-machine ssh myvm1 "docker node ls" #List nodes in this swarm`

### Clean up

`docker stack rm getstartedlab #Delete stack`
`docker stack rm <appname>                             # Tear down an application`
`docker swarm leave --force      # Take down a single node swarm from the manager`
`docker-machine stop $(docker-machine ls -q) # Stop all running VMs`
`docker-machine rm $(docker-machine ls -q) # Delete all VMs and their disk images`
`docker image rm <image id>            # Remove specified image from this machine`
`docker image rm $(docker image ls -a -q)   # Remove all images from this machine`
`docker container rm $(docker container ls -a -q)         # Remove all containers`
