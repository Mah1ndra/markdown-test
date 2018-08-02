# Scenario-9: Docker Networks

>This scenario explores how to create a docker network allowing containers to communicate. We'll also explore the Embedded DNS Server added in Docker 1.10.

>Docker has two approaches to networking. The first defines a link between two containers. This link updates /etc/hosts and environment variables to allow containers to discover and communicate.

>The alternate approach is to create a docker network that containers are connected too. The network has similar attributes to a physical network, allowing containers to come and go more freely than when using links.
---

## Step 1 - Create Network
>command to create a network
```
docker network create backend-network
```
>When we launch new containers, we can use the --net attribute to assign which network they should be connected to. 
```bash
docker run -d --name=redis --net=backend-network redis
```
---
## Step 2 - Network Communication
>Unlike using links, docker network behave like traditional networks where nodes can be attached/detached.

>The first thing you'll notice is that Docker no longer assigns environment variables or updates the hosts file of containers. Explore using the following two commands and you'll notice it no longer mentions other containers.
```bash
docker run --net=backend-network alpine env
```
```bash
docker run --net=backend-network alpine cat /etc/hosts
```
>Instead, the way containers can communicate via an Embedded DNS Server in Docker. This DNS server is assigned to all containers via the IP 127.0.0.11 and set in the resolv.conf file.

```bash
docker run --net=backend-network alpine cat /etc/resolv.conf
```
>When containers attempt to access other containers via a well-known name, such as Redis, the DNS server will return the IP address of the correct Container. In this case, the fully qualified name of Redis will be redis.backend-network.
```bash
docker run --net=backend-network alpine ping -c1 redis
```
---
## Step 3 - Connect Two Containers
>The first task is to create a new network in the same way.
```bash
docker network create frontend-network
```
>When using the connect command it is possible to attach existing containers to the network.
```bash
docker network connect frontend-network redis
```
>When we launch the web server, given it's attached to the same network it will be able to communicate with our Redis instance
```bash
docker run -d -p 3000:3000 --net=frontend-network katacoda/redis-node-docker-example
```
>You can test it using
```bash
curl docker:3000
```
---
## Step 4 - Create Aliases
>The following command will connect our Redis instance to the frontend-network with the alias of db.
```bash
docker network create frontend-network2
docker network connect --alias db frontend-network2 redis
```
>When containers attempt to access a service via the name db, they will be given the IP address of our Redis container.

```bash
docker run --net=frontend-network2 alpine ping -c1 db
```
---
## Step 5 - Disconnect Containers
>The following command will list all the networks on our host.
```bash
docker network ls
```
>We can then explore the network to see which containers are attached and their IP addresses.
```bash
docker network inspect frontend-network
```
>The following command disconnects the redis container from the frontend-network.


```bash
docker network disconnect frontend-network redis
```
---