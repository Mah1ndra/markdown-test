# Scenario-11: Managing Log Files

>This scenario will explore the different ways you can handle logging output from your application and services when running as containers.
---
## Step 1 - Docker Logs
>When you start a container, Docker will track the Standard Out and Standard Error outputs from the process and make them available via the client.

>Using the Docker client, we can access the standard out and standard error outputs using 
```bash
docker logs <container name/id>
```
---
## Step 2 - SysLog
>The Syslog log driver will write all the container logs to the central syslog on the host. "syslog is a widely used standard for message logging. It permits separation of the software that generates messages, the system that stores them, and the software that reports and analyses them.

>This log-driver is designed to be used when syslog is being collected and aggregated by an external system.

>The command below will redirect the redis logs to syslog.
```bash
docker run -d --name redis-syslog --log-driver=syslog redis
``` 
---
## Step 3 - Disable Logging
>The third option is to disable logging on the container. This is particularly useful for containers which are very verbose in their logging.

>When the container is launched simply set the log-driver to none. No output will be logged.
```bash
docker run -d --name redis-none --log-driver=none redis
```
>The inspect command allows you to identify the logging configuration for a particular container. The command below will output the LogConfig section for each of the containers.

>Server created in step 1
```bash
docker inspect --format '{{ .HostConfig.LogConfig }}' redis-server
```
>Server created in step 2
```bash
docker inspect --format '{{ .HostConfig.LogConfig }}' redis-syslog
```
>Server created in this step
```bash
docker inspect --format '{{ .HostConfig.LogConfig }}' redis-none
```
---