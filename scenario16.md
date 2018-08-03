# Scenario-16: Docker Stats

>When running containers in production, it's important to monitor there runtime metrics, such as CPU usage and memory, to ensure they're behaving as expected. These metrics can also help diagnose issues if they occur.

>In this scenario, we'll explore the built-in metrics provided by Docker to give additional visibility to the running containers.
---
## Step 1 - Single Container
> Start a container
```bash
docker run -d -p 80:80 -e DEFAULT_HOST=proxy.example -v /var/run/docker.sock:/tmp/docker.sock:ro --name nginx jwilder/nginx-proxy:alpine
```
>You can find the stats for the container by using:
```bash
docker stats nginx
```
---
##
>we can take the list of all our running containers provided by docker ps and use them as the argument for docker stats. This gives us an overview of the entire machine's containers.
```bash
docker ps -q | xargs docker stats
```
---