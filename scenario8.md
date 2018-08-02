# Scenario-8: Communicating Between Containers

>This scenario explores how to allow multiple containers to communicate with each other. The steps will explain how to connect a data-store, in this case, Redis, to an application running in a separate container.
---
## Step 1 - Start Redis
>Run a redis server with a friendly name of redis-server which we'll connect to in the next step. This will be our source container.
```bash
docker run -d --name redis-server redis
```
---
## Step 2 - Create Link
>To connect to a source container you use the --link <container-name|id>:<alias> option when launching a new container. The container name refers to the source container we defined in the previous step while the alias defines the friendly name of the host.

>First, Docker will set some environment variables based on the linked to the container. These environment variables give you a way to reference information such as Ports and IP addresses via known names.

>You can output all the environment variables with the env command. For example:
```bash
docker run --link redis-server:redis alpine env
```
>Secondly, Docker will update the HOSTS file of the container with an entry for our source container with three names, the original, the alias and the hash-id. You can output the containers host entry using cat /etc/hosts
```bash
docker run --link redis-server:redis alpine cat /etc/hosts
```
>With a link created you can ping the source container in the same way as if it were a server running in your network.
```bash
docker run --link redis-server:redis alpine ping -c 1 redis
```
---
## Step 3 - Connect To App
>Here is a simple node.js application which connects to redis using the hostname redis.
```bash
docker run -d -p 3000:3000 --link redis-server:redis katacoda/redis-node-docker-example
```
>Sending an HTTP request to the application will store the request in Redis and return a count. If you issue multiple requests, you'll see the counter increment as items are persisted.
```bash
curl docker:3000
```
---
## Step 4 - Connect to Redis CLI
>The command below will launch an instance of the Redis-cli tool and connect to the redis server via it's alias.
```bash
docker run -it --link redis-server:redis redis redis-cli -h redis
```
>The command KEYS * will output the contents stored currently in the source redis container.

>Type QUIT to exit the CLI.
---