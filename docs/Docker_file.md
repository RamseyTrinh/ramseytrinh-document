# Dockerfile cheat sheet

### DOCKERFILE PARTS

- FROM - The os used. Common is alpine, debian, ubuntu
- ENV - Environment variables
- RUN - Run commands/shell scripts, etc
- EXPOSE - Ports to expose
- CMD - Final command run when you launch a new container from image
- WORKDIR - Sets working directory (also could use 'RUN cd /some/path')
- COPY # Copies files from host to container

### Build image from dockerfile (reponame can be whatever)

### From the same directory as Dockerfile

```
$ docker image build -t [REPONAME] .
```
### Benchmarking builds

```
$ DOCKER_BUILDKIT=1 docker image build -t [REPONAME] .
```

#### TIP: CACHE & ORDER

- If you re-run the build, it will be quick because everythging is cached.
- If you change one line and re-run, that line and everything after will not be cached
- Keep things that change the most toward the bottom of the Dockerfile

# EXTENDING DOCKERFILE

### Build image from Dockerfile

```
$ docker image build -t nginx-website
```

### Running it

```
$ docker container run -p 80:80 --rm nginx-website
```

### Tag and push to Dockerhub

```
$ docker image tag nginx-website:latest btraversy/nginx-website:latest
```

```
$ docker image push bradtraversy/nginx-website
```

# VOLUMES

### Volume - Makes special location outside of container UFS. Used for databases

### Bind Mount -Link container path to host path

### Check volumes

```
$ docker volume ls
```

### Cleanup unused volumes

```
$ docker volume prune
```

### Pull down mysql image to test

```
$ docker pull mysql
```

### Inspect and see volume

```
$ docker image inspect mysql
```

### Run container

```
$ docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysql
```

### Inspect and see volume in container

```
$ docker container inspect mysql
```

#### TIP: Mounts

- You will also see the volume under mounts
- Container gets its own uniqe location on the host to store that data
- Source: xxx is where it lives on the host

### Check volumes

```
$ docker volume ls
```

**There is no way to tell volumes apart for instance with 2 mysql containers, so we used named volumes**

### Named volumes (Add -v command)(the name here is mysql-db which could be anything)

```
$ docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql
```

### Inspect new named volume

```bash
docker volume inspect mysql-db
```

# BIND MOUNTS

- Can not use in Dockerfile, specified at run time (uses -v as well)
- ... run -v /Users/brad/stuff:/path/container (mac/linux)
- ... run -v //c/Users/brad/stuff:/path/container (windows)

**TIP: Instead of typing out local path, for working directory use $(pwd):/path/container - On windows may not work unless you are in your users folder**

### Run and be able to edit index.html file (local dir should have the Dockerfile and the index.html)

```
$ docker container run  -p 80:80 -v $(pwd):/usr/share/nginx/html nginx
```

### Go into the container and check

```bash
$ docker container exec -it nginx bash
$ cd /usr/share/nginx/html
$ ls -al
```

### You could create a file in the container and it will exiost on the host as well

```bash
$ touch test.txt
```

# Link

```bash
$ docker run -d --name my-postgres postgres
```
now Link it with dot net

```bash
$ docker run -d -p 5000:5000 --link my-postgres:postgres btree/dotnet
```


# DOCKER COMPOSE

- Configure relationships between containers
- Save our docker container run settings in easy to read file
- 2 Parts: YAML File (docker.compose.yml) + CLI tool (docker-compose)

### 1. docker.compose.yml - Describes solutions for

- containers
- networks
- volumes

### 2. docker-compose CLI - used for local dev/test automation with YAML files

### Sample compose file (From Bret Fishers course)

```
version: '2'

# same as
# docker run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve

services:
  jekyll:
    image: bretfisher/jekyll-serve
    volumes:
      - .:/site
    ports:
      - '80:4000'
```


### Example 2
```
version: '3'
services:
  app:
    container_name: docker-node-mongo
    restart: always
    build: .
    ports:
      - '80:3000'
    links:
      - mongo
  mongo:
    container_name: mongo
    image: mongo
    ports:
      - '27017:27017'
```

### To run

```
docker-compose up
```

### You can run in background with

```
docker-compose up -d
```

### To cleanup

```
docker-compose down
```

#### [this has been taken from](https://gist.github.com/bradtraversy/89fad226dc058a41b596d586022a9bd3)
