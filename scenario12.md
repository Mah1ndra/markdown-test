# Scenario-12: Ensuring Uptime

>In the previous scenarios we've launched some containers, but like any process, containers can crash. This scenario will explore how you can keep containers live and automatically restart them if the crash unexpectedly.
--- 
## Step 1 - Stop On Fail
>Docker considers any containers to exit with a non-zero exit code to have crashed. By default a crashed container will remain stopped.

>You can launch an instance using 
```bash
docker run -d --name restart-default scrapbook/docker-restart-example
```
>If you list all the containers, including stopped, you will see the container has crashed 
```bash
docker ps -a
```
>While the logs will output our message, which in real-life would hopefully indicate information to help us diagnose the issue.
```bash
docker logs restart-default
```
---
## Step 2 - Restart On Fail
>The option --restart=on-failure:# allows you to say how many times Docker should try again. In the example below, Docker will restart the container three times before stopping.
```bash
docker run -d --name restart-3 --restart=on-failure:3 scrapbook/docker-restart-example
```
>As we can see from the logs, it was launched on three occasions. 
```bash
docker logs restart-3
```
---

## Step 3 - Always Restart
>Finally Docker can always restart a failed container, in this case, Docker will keep trying until the container it is explicitly told to stop.

>Use the always flag to automatically restart the container when is crashes for example 
```bash
docker run -d --name restart-always --restart=always scrapbook/docker-restart-example
```

>You can view the restart attempting via the log 
```bash
docker logs restart-always
```
---