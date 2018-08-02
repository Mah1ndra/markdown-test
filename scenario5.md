# Scenario-5: Optimising Dockerfile with OnBuild 
## Step 1 - OnBuild
>Below is the Node.js OnBuild Dockerfile.
```docker
FROM node:7
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
ONBUILD COPY package.json /usr/src/app/
ONBUILD RUN npm install
ONBUILD COPY . /usr/src/app
CMD [ "npm", "start" ]
```
---
## Step 2 - Application Dockerfile
>the only aspect which needs to be defined on the application level is which port(s) to expose.
```docker
FROM node:7-onbuild
EXPOSE 3000
```
---
## Step 2 - Building & Launching Container
>The command to build the image is 
```docker
docker build -t my-nodejs-app .
```
>The command to launch the built image is 
```docker
docker run -d --name my-running-app -p 3000:3000 my-nodejs-app
```
>You can test the container is accessible using curl
```bash
curl http://docker:3000
```
---
