# Scenario-19: Getting Started With Swarm Mode

>In this scenario, you will learn how to initialise a Docker Swarm Mode cluster and deploy networked containers using the built-in Docker Orchestration. The environment has been configured with two Docker hosts.

>In 1.12, Docker introduced Swarm Mode. Swarm Mode enables the ability to deploy containers across multiple Docker hosts, using overlay networks for service discovery with a built-in load balancer for scaling the services.

>Swarm Mode is managed as part of the Docker CLI, making it a seamless experience to the Docker ecosystem.
---
## Step 1 - Initialise Swarm Mode
>Swarm Mode is built into the Docker CLI. You can find an overview the possibility commands via
```bash
docker swarm --help
```
>The most important one is how to initialise Swarm Mode. Initialisation is done via init.
```bash
docker swarm init
```
---
## Step 2 - Join Cluster
>The first task is to obtain the token required to add a worker to the cluster. For demonstration purposes, we'll ask the manager what the token is via swarm join-token. In production, this token should be stored securely and only accessible by trusted individuals.
```bash
token=$(docker -H 172.17.0.19:2345 swarm join-token -q worker) && echo $token
```

>On the second host, join the cluster by requesting access via the manager. The token is provided as an additional parameter.
```bash
docker swarm join 172.17.0.19:2377 --token $token
```
>By default, the manager will automatically accept new nodes being added to the cluster. You can view all nodes in the cluster using 
```bash
docker node ls
```
---
## Step 3 - Create Overlay Network
>The following command will create a new overlay network called skynet. All containers registered to this network can communicate with each other, regardless of which node they are deployed onto.
```bash
docker network create -d overlay skynet
```
---
## Step 4 - Deploy Service
>Finally, we load balance these two containers together on port 80. Sending an HTTP request to any of the nodes in the cluster will process the request by one of the containers within the cluster. The node which accepted the request might not be the node where the container responses. Instead, Docker load-balances requests across all available containers.
```bash
docker service create --name http --network skynet --replicas 2 -p 80:80 katacoda/docker-http-server
```
>You can view the services running on the cluster using the CLI command
```bash
docker service ls
```
>As containers are started you will see them using the ps command. You should see one instance of the container on each host.
```bash
docker ps
```
>As containers are started you will see them using the ps command. You should see one instance of the container on each host.
```bash
curl docker
```
---
## Step 5 - Inspect State
>You can view the list of all the tasks associated with a service across the cluster. In this case, each task is a container 
```bash
docker service ps http
```
>You can view the details and configuration of a service via 
```bash
docker service inspect --pretty http
```
>On each node, you can ask what tasks it is currently running. Self refers to the manager node Leader:
```bash
docker node ps self
```
>Using the ID of a node you can query individual hosts 
```bash
docker node ps $(docker node ls -q | head -n1)
```
---
## Step 6 - Scale Service
>A Service allows us to scale how many instances of a task is running across the cluster. As it understands how to launch containers and which containers are running, it can easily start, or remove, containers as required. At the moment the scaling is manual. However, the API could be hooked up to an external system such as a metrics dashboard.

>At present, we have two load-balanced containers running, which are processing our requests 
```bash
curl docker
```
>The command below will scale our http service to be running across five containers.
```bash
docker service scale http=5
```
>On each host, you will see additional nodes being started 
```bash
docker ps
```
>The load balancer will automatically be updated. Requests will now be processed across the new containers. Try issuing more commands via 
```bash
curl docker
```
---
