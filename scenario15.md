# Scenario-15: Orchestration using Docker Compose

>When working with multiple containers, it can be difficult to manage the starting along with the configuration of variables and links. To solve this problem, Docker has a tool called Docker Compose to manage the orchestration, of launching, of containers.
---
## Step 1 - Defining First Container
>Docker Compose is based on a docker-compose.yml file. This file defines all of the containers and settings you need to launch your set of clusters. The properties map onto how you use the docker run commands, however, are now stored in source control and shared along with your code.

>Copy the following yaml into the editor. This will define a container called web, which is based on the build of the current directory.
```yaml
web:
  build: .
```
---
## Step 2 - Defining Settings
>To link two containers together to specify a links property and list required connections. For example, the following would link to the redis source container defined in the same file and assign the same name to the alias.
```yaml
links:
    - redis
```
>The same format is used for other properties such as ports
```yaml
  ports:
    - "3000"
    - "8000"
```
---
## Step 3 - Defining Second Container
>Define the second container with the name redis which uses the image redis. Following the YAML format, the container details would be:
```yaml
Editorredis:
  image: redis:alpine
  volumes:
    - /var/redis/data:/data
```
---
>With the created docker-compose.yml file in place, you can launch all the applications with a single command of up. If you wanted to bring up a single container, then you can use up <name>.

>The -d argument states to run the containers in the background, similar to when used with docker run.
```bash
docker-compose up -d
```
---
## Step 5 - Docker Management
>Not only can Docker Compose manage starting containers but it also provides a way manage all the containers using a single command.

>to see the details of the launched containers
```bash
docker-compose ps
```
>To access all the logs via a single stream 
```bash
docker-compose logs
```
>Other commands follow the same pattern. Discover them by typing
```bash
docker-compose
```
---
## Step 6 - Docker Scale
>Scale the number of web containers you're running using the command 
```bash
docker-compose scale web=3
```
>You can scale it back down using 
```bash
docker-compose scale web=1
```
---
## Step 7 - Docker Stop
>As when we launched the application, to stop a set of containers you can use the command 
```bash
docker-compose stop
```
>To remove all the containers use the command 
```bash
docker-compose rm
```
---
