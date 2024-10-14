# Docker cheat sheet

## Docker Commands, Help & Tips

### Docker version

```shell
$ docker version
```

### Show info

```shell
$ docker info
```

### IMAGE COMMANDS

### List images

```
$ docker image ls
or
$ docker images
```

### Pull down images

```
$ docker pull [IMAGE]
```

### Remove image

```
$ docker image rm [IMAGE]
$ docker rmi [IMAGE]
```

### Remove all images

```
$ docker rmi $(docker images -a -q)
```

### Some sample container creation

NGINX:

```bash
$ docker container run -d -p 80:80 --name nginx nginx
```

APACHE:

```bash
$ docker container run -d -p 8080:80 --name apache httpd
```

MONGODB:

```bash
$ docker container run -d -p 27017:27017 --name mongo mongo
```

MYSQL:

```bash
$ docker container run -d -p 3306:3306 --name mysql --env MYSQL_ROOT_PASSWORD=123456 mysql
```
###  CONTAINERS

### Create and run

```bash
$ docker container run -it -p 80:80 nginx
```

### Some flag in command run Containers

```bash
-d: run in "detached" mode
-i: keep STDIN open 
-t: allocate a terminal interating with container
-it: access terminal
--name: name
-p: map ports
--rm: remove the container when it exits.
-v: mout a volume
--network: specify the network (Default: bridge)
-e: environment variables
--link: link to another running container
--restart: set the restart policy
```

### List running containers

```
$ docker ps
```

### List all containers (Even if not running)

```
$ docker ps -a
```

### Stop container

```
$ docker container stop []
```

### Stop all running containers

```
$ docker stop $(docker ps -aq)
```

### To remove a running container use force(-f)

```
$ docker container rm -f [ID]
```

### Remove multiple containers

```
$ docker container rm [ID] [ID] [ID]
```

### Remove all containers

```
$ docker rm $(docker ps -aq)
```

### Get logs (Use name or ID)

```
$ docker container logs [NAME]
```

### List processes running in container

```
$ docker container top [NAME]
```

#### TIP: ABOUT CONTAINERS

Docker containers are often compared to virtual machines but they are actually just processes running on your host os. In Windows/Mac, Docker runs in a mini-VM so to see the processes youll need to connect directly to that. On Linux however you can run "ps aux" and see the processes directly

### View info on container

```
$ docker container inspect [NAME]
```

### Performance stats (cpu, mem, network, disk, etc)

```
$ docker container stats [NAME]
```

### Access an already created container

```
$ docker container start -ai ubuntu
```

### Edit config

```bash
$ docker container exec -it mysql bash
```

# Limiting The Memory Usage For Containers

In order to limit the amount of memory a docker container process can use, simply set the -m [memory amount] flag with the limit.

To run a container with memory limited to 256 MBs:

##### Example: docker run -name [name] -m [Memory (int)][memory unit (b, k, m or g)] -d (to run not to attach) -p (to set access and expose ports) [image ID]
```
$ docker run -m 64m -d -p 8082:80 tutum/wordpress
```

# NETWORKING

### "bridge" or "docker0" is the default network

### Get port

```
$ docker container port [NAME]
```

### List networks

```
$ docker network ls
```

### Inspect network

```
$ docker network inspect [NETWORK_NAME]
("bridge" is default)
```

### Create network

```
$ docker network create [NETWORK_NAME]
```

or 
```bash
$ docker network create --driver bridge [NETWORK_NAME]
```

### Connect existing container to network

```
$ docker network connect [NETWORK_NAME] [CONTAINER_NAME]
```

### Disconnect container from network

```
$ docker network disconnect [NETWORK_NAME] [CONTAINER_NAME]
```

### Detach network from container

```
$ docker network disconnect
```

### Delete/Remove network

To remove the network by name or id, multiple can be deleted:

```shell
$ docker network rm [NETWORK_NAME] [NETWORK_NAME]
```

# IMAGE TAGGING & PUSHING TO DOCKERHUB

```
$ docker image ls
```

### Retag existing image

```
$ docker image tag nginx btraversy/nginx
```

### Upload to dockerhub

```
$ docker image push bradtraversy/nginx
```