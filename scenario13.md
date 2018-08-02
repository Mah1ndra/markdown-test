# Scenario-13: Docker Metadata & Labels

>When running containers in production, it can be useful to add additional metadata relating to the container to help their management. This metadata could be related to which version of the code is running, which applications or users own the container or define special criteria such as which servers they should run on.

>This additional data is managed via Docker Labels. Labels allow you to define custom metadata about a container or image which can later be inspected or used as part of a filter.

---
## Step 1 - Docker Containers
>To add a single label you use the l =<value> option. The example below assigns a label called user with an ID to the container. This would allow us to query for all the containers running related to that particular user.
```bash
docker run -l user=12345 -d redis
```
>If you're adding multiple labels, then these can come from an external file. The file needs to have a label on each line, and then these will be attached to the running container.


>This line creates two labels in the file, one for the user and the second assigning a role.
```bash
echo 'user=123461' >> labels && echo 'role=cache' >> labels
```
>The --label-file=<filename> option will create a label for each line in the file.
```bash
docker run --label-file=labels -d redis
```
---
## Step 2 - Docker Images
>Within a Dockerfile you can assign a label using the LABEL instruction. Below the label vendor is created with the name Scrapbook.
```text
LABEL vendor=Katacoda
```
>If we want to assign multiple labels then, we can use the format below with a label on each line, joined using a back-slash ("\"). Notice we're using the DNS notation format for labels which are related to third party tooling.
```docker
LABEL vendor=Katacoda \ com.katacoda.version=0.0.5 \ com.katacoda.build-date=2016-07-01T10:47:29Z \ com.katacoda.course=Docker
```
---
## Step 3 - Inspect
>By providing the running container's friendly name or hash id, you can query all of it's metadata.
```bash
docker inspect rd
```
>Using the -f option you can filter the JSON response to just the Labels section we're interested in.

```bash
docker inspect -f "{{json .Config.Labels }}" rd
```
>Inspecting images works in the same way however the JSON format is slightly different, naming it ContainerConfig instead of Config.
```bash
docker inspect -f "{{json .ContainerConfig.Labels }}" katacoda-label-example
```
---
## Step 4 - Query By Label
>The docker ps command allows you to specify a filter based on a label name and value. For example, the query below will return all the containers which have a user label key with the value katacoda.
```bash
docker ps --filter "label=user=scrapbook"
```
>The same filter approach can be applied to images based on the labels used when the image was built.
```bash
docker images --filter "label=vendor=Katacoda"
```
---
## Step 5 - Daemon labels
>Labels are not only applied to images and containers but also the Docker Daemon itself. When you launch an instance of the daemon, you can assign it labels to help identify how it should be used, for example, if it's a development or production server or if it's more suited to particular roles such running databases.
```bash
docker -d \
-H unix:///var/run/docker.sock \
--label com.katacoda.environment="production" \
--label com.katacoda.storage="ssd"
```
---
