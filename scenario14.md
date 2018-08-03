# Scenario-14: Load Balancing Containers

>In this session, we'll explore how you can use the NGINX web server to load balance requests between two containers running on the host.

>With Docker, there are two main ways for containers to communicate with each other. The first is via links which configure the container with environment variables and host entry allowing them to communicate. The second is using the Service Discovery pattern where uses information provided by third parties, in this scenario, it will be Docker's API.

>The Service Discovery pattern is where the application uses a third party system to identify the location of the target service. For example, if our application wanted to talk to a database, it would first ask an API what the IP address of the database is. This pattern allows you to quickly reconfigure and scale your architectures with improved fault tolerance than fixed locations.

---
## Step 1 - NGINX Proxy
>Use the command below to launch nginx-proxy.
```bash
docker run -d -p 80:80 -e DEFAULT_HOST=proxy.example -v /var/run/docker.sock:/tmp/docker.sock:ro --name nginx jwilder/nginx-proxy
```
>Because we're using a DEFAULT_HOST, any requests which come in will be directed to the container that has been assigned the HOST proxy.example.

>You can make a request to the web server using
```bash
curl http://docker
```
> As we have no containers, it will return a 503 error.
---
## Step 2 - Single Host
>For Nginx-proxy to start sending requests to a container you need to specify the VIRTUAL_HOST environment variable. This variable defines the domain where requests will come from and should be handled by the container.

>In this scenario we'll set our HOST to match our DEFAULT_HOST so it will accept all requests.

```bash
docker run -d -p 80 -e VIRTUAL_HOST=proxy.example katacoda/docker-http-server
```
>Sometimes it takes a few seconds for NGINX to reload but if we execute a request to our proxy using 
```bash
curl http://docker
```
> then the request will be handled by our container.
---
## Step 3 - Cluster
>We now have successfully created a container to handle our HTTP requests. If we launch a second container with the same VIRTUAL_HOST then nginx-proxy will configure the system in a round-robin load balanced scenario. This means that the first request will go to one container, the second request to a second container and then repeat in a circle. There is no limit to the number of nodes you can have running.

>Launch a second container using the same command as we did before.
```bash
docker run -d -p 80 -e VIRTUAL_HOST=proxy.example katacoda/docker-http-server
```
> Send the request to our proxy and check
```bash
curl http://docker
```
##  Step 4 - Generated NGINX Configuration
>While nginx-proxy automatically creates and configures NGINX for us, if you're interested in what the final configuration looks like then you can output the complete config file with docker exec as shown below.
```bash
docker exec nginx cat /etc/nginx/conf.d/default.conf
```
>Additional information about when it reloads configuration can be found in the logs using 
```docker
docker logs nginx
```
---
