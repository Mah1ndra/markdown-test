# Scenario-10: Persisting Data Using Volumes


>In this scenario, you'll learn how to use Docker Volumes to persistent data within Containers. Docker Volumes allow directories to be shared between containers and container versions.

>Docker Volumes allows you to upgrade containers, restart machines and share data without data loss. This is essential when updating database or application versions.
---
## Step 1 - Data Volumes
>This example will use Redis as a way to persist data. Start a Redis container below, and create a data volume using the -v parameter. This specifies that any data saved inside the container to the /data directory should be persisted on the host in the directory /docker/redis-data.
```bash
docker run  -v /docker/redis-data:/data --name r1 -d redis  redis-server --appendonly yes
```
>We can pipe data into the Redis instance using the following command.
```bash
cat data | docker exec -i r1 redis-cli --pipe
```
>Redis will save this data to disk. On the host we can investigate the mapped direct which should contain the Redis data file.
```bash
ls /docker/redis-data
```
>This same directory can be mounted to a second container. One usage is to have a Docker Container performing backup operations on your data.
```bash
docker run  -v /docker/redis-data:/backup ubuntu ls /backup
```
---
## Step 2 - Shared Volumes
>In this case, we're mapping our Redis container's volume to an Ubuntu container. The /data directory only exists within our Redis container, however, because of -volumes-from our Ubuntu container can access the data.
```bash
docker run --volumes-from r1 -it ubuntu ls /data
```
---
## Step 3 - Read-only Volumes
>Mounting Volumes gives the container full read and write access to the directory. You can specify read-only permissions on the directory by adding the permissions :ro to the mount.

>If the container attempts to modify data within the directory it will error.

```bash
docker run -v /docker/redis-data:/data:ro -it ubuntu rm -rf /data
```