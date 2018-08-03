# Scenario-18: Formatting PS Output

>In this scenario you will learn how to use the _--format__ parameters to pretty-print output from docker ps and docker inspect. 
---
## Example 1 - Names and Images as Table
>The format of docker ps can be formatted to only display the information relevant to you.

>Start by launching a example container - 
```bash
docker run -d redis
```
>The standard docker ps command outputs the name, image used, command, uptime and port information.

>To limit which columns are displayed, use the _--format__ parameter. The parameter allows pretty-printing containers using a Go template syntax.

```docker
docker ps --format '{{.Names}} container is using {{.Image}} image'
```
>As it's using Go templates, it includes helper functions such as table.
```docker
docker ps --format 'table {{.Names}}\t{{.Image}}'
```
---
## Example 2 - List IP addresses
>Thankfully, the docker inspect also supports pretty-printing the results via a Go Template. The container IDs from docker ps can be piped into docker inspect.

>The format parameter can then access all of the container information. Below is an example of listing all the IP addresses for the running containers.

```docker
docker ps -q | xargs docker inspect --format '{{ .Id }} - {{ .Name }} - {{ .NetworkSettings.IPAddress }}'
```
---