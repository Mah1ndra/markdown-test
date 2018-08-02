# Scenario: Deploying Your First Docker Container 

## Step 1 - Running A Container
> Search for docker images
```bash
docker search redis
```


> To run the docker in background, the option -d needs to be specified.
```bash
 docker run -d redis
  ```
  ---

## Step 2 - Finding Running Containers

> To lists all running containers
```bash
docker ps
```
>To know more details about a running container, such as IP address.
```bash
docker inspect <friendly-name|container-id>
```
>To get logs of a container
```bash
docker logs <friendly-name|container-id>
```
---

## Step 3 - Accessing Redis
>To start redis container 
```bash
docker run -d --name redisHostPort -p 6379:6379 redis:latest
```
>just using the option -p 6379 enables her to expose Redis but on a randomly available port
```bash
docker run -d --name redisDynamic -p 6379 redis:latest
```
> To know which port has been assigned
```bash
docker port redisDynamic 6379
```
> **docker ps** displays the port mapping information
```bash
docker ps
```
---
## Step 4 - Persisting Data
>Any data which needs to be saved on the Docker Host, and not inside containers, should be stored in /opt/docker/data/redis.
```bash
docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis
```

---
## Step 5 - Running A Container In The Foreground
> Without specifing -d(--detach) option the container run in foregroud

> The following launches an Ubuntu container and executes the command ps to view all the processes running in a container.
```bash
docker run ubuntu ps
```
>To access a bash shell inside a ubuntu container
```bash
docker run -it ubuntu bash
```
---