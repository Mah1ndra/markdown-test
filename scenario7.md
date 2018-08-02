# Scenario-7: Data Containers
>There are two ways of approaching stateful Containers, that is containers are store and persistent data for future use. This could be the container creating and storing data, for example, a database. Alternatively, it could be data requiring additional for instance the configuration or SSL certifications. This approach can also be used to backup data or debug containers.
---
# Step 1 - Create Container
>To create a Data Container we first create a container with a well-known name for future reference. We use busybox as the base as it's small and lightweight in case we want to explore and move the container to another host.
>When creating the container, we also provide a -v option to define where other containers will be reading/saving data.
>Create a Data Container for storing 

>configuration files using 
```bash
docker create -v /config --name dataContainer busybox
```
---
# Step 2 - Copy Files

>To copy files into a container you use the command docker cp. The following command will copy the config.conf file into our dataContainer and the directory config.
```bash
docker cp config.conf dataContainer:/config/
```
>Using the --volumes-from <container> option we can use the mount volumes from other containers inside the container being launched. In this case, we'll launch an Ubuntu container which has reference to our Data Container. When we list the config directory, it will show the files from the attached container.
```bash
docker run --volumes-from dataContainer ubuntu ls /config
```
---
# Step 4 - Export / Import Containers
>If we wanted to move the Data Container to another machine then we can export it to a .tar file.
```bash
docker export dataContainer > dataContainer.tar
```
>The following command will import the Data Container back into Docker.
```bash
docker import dataContainer.tarv
```
---

