#Scenario-2: Deploy Static HTML Website as Container
## Step 1 - Create Dockerfile
>Step 1 - Create Dockerfile
```bash
FROM nginx:alpine
COPY . /usr/share/nginx/html
```
>The first line defines our base image. The second line copies the content of the current directory into a particular location inside the container.
---
## Step 2 - Build Docker Image
> Build our static HTML image using the build command below.
```bash
docker build -t webserver-image:v1 .
```
>You can view a list of all the images on the host using 
```bash
docker images
```
---
## Step 3 - Run
>Launch our newly built image providing the friendly name and tag. As it's a web server, bind port 80 to our host using the -p parameter.
```bash
docker run -d -p 80:80 webserver-image:v1
```
>Once started, you'll be able to access the results of port 80 via
```
curl docker
```
---
