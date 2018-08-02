# Scenario-2: Building Container Images

## Step 1 - Base Images
> Add the following line in **Dockerfile**
```bash
FROM nginx:1.11-alpine
```
---
## Step 2 - Running Commands
>A new index.html file has been created for you which we want to serve from our container. On the next line after the FROM command, use the COPY command to copy index.html into a directory called /usr/share/nginx/html

```bash
Copy to EditorCOPY index.html /usr/share/nginx/html/index.html
```
---
## Step 3 - Exposing Ports
>We want our web server to be accessible via port 80, add the relevant EXPOSE line to the Dockerfile.
```bash
EXPOSE 80
```
---
## Step 4 - Default Commands
>The command to run NGINX is nginx -g daemon off;. Set this as the default command in the Dockerfile.
```bash
Copy to EditorCMD ["nginx", "-g", "daemon off;"]
```

---
## Step 5 - Building Containers
>Using the docker build command to build the image. You can give the image a friendly name by using the -t <name> option.
```bash
docker build -t my-nginx-image:latest .
```
---
## Step 6 - Launching New Image
>NGINX is designed to run as a background service so you should include the option -d. To make the web server accessible, bind it to port 80 using p 80:80
```bash
docker run -d -p 80:80 my-nginx-image:latest
```
--- 