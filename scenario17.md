# Scenario-17: Creating optimised Docker Images using Multi-Stage Builds  
>In this scenario you will learn how to use the multi-stage build functionality to make smaller, more optimised images.

>The feature is ideal for deploying languages such as Golang as containers. By having multi-stage builds, the first stage can build the Golang binary using a larger Docker image as the base. In the second stage, the newly built binary can be deployed using a much smaller base image. The end result is an optimised Docker Image.
---
## Create Dockerfile
>Start by deploying a sample Golang HTTP Server. This currently using a two staged Docker Build approach. This scenario will create a new Dockerfile that allows the image to be built using a single command.
```bash
git clone https://github.com/katacoda/golang-http-server.git
```
> create a Multi-Stage Dockerfile. The first stage using the Golang SDK to build a binary. The second stage copies the resulting binary into a optimised Docker Image.
```docker
# First Stage
FROM golang:1.6-alpine

RUN mkdir /app
ADD . /app/
WORKDIR /app
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

# Second Stage
FROM alpine
EXPOSE 80
CMD ["/app"]

# Copy from first stage
COPY --from=0 /app/main /app
```
---
## 
>Create the desired Docker Image using the build command below.
```bash
docker build -f Dockerfile.multi -t golang-app .
```
>The result will be two images. One untagged that was used for the first stage and the second, smaller image, our target image.
```bash
docker images
```
>If you receive the error, COPY --from=0 /build/out /app/ Unknown flag: from, it means you're running an older version of Docker without the multi-stage support. Step 1 of this scenario upgrades the current Docker version.
---
## step-3: Test Image
>The image can be launched and deployed without any changes required.
```bash
docker run -d -p 80:80 golang-app
```
```bash
curl localhost
```
---