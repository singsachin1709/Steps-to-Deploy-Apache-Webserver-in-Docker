# Steps-to-Deploy-Apache-Webserver-in-Docker
## 1. Install Docker

Make sure Docker is installed and running on your system.
Check:
```
docker --version
```
## 2. Pull Apache HTTPD Image

Docker Hub provides an official Apache image called httpd.
```
docker pull httpd:latest
```
## 3. Run Apache Container

Run a container with port mapping (host:container → 8080:80):
```
docker run -d --name apache-server -p 8080:80 httpd:latest
```
- ```-d``` → run in detached mode
- ```--name apache-server``` → container name
- ```-p 8080:80``` → map host port 8080 to container port 80

👉 Now, open your browser and go to:
http://localhost:8080 (or ```http://<your-server-ip>:8080```)

## 4. Serve Your Custom HTML Pages

By default, Apache serves content from /usr/local/apache2/htdocs inside the container.
To mount your local HTML files:
```
docker run -d --name apache-server -p 8080:80 \
  -v $(pwd)/my-website:/usr/local/apache2/htdocs \
  httpd:latest
```
Replace $(pwd)/my-website with the path to your local website files.

## 5. Customize Apache Configuration (Optional)

The Apache config file is located at:
```
/usr/local/apache2/conf/httpd.conf
```
If you want to use your own configuration:
```
docker run -d --name apache-server -p 8080:80 \
  -v $(pwd)/httpd.conf:/usr/local/apache2/conf/httpd.conf \
  -v $(pwd)/my-website:/usr/local/apache2/htdocs \
  httpd:latest
```
## 6. Using a Dockerfile (Custom Apache Image)

If you want to build your own image with website + config:
```
# Dockerfile
FROM httpd:latest
COPY ./my-website/ /usr/local/apache2/htdocs/
COPY ./httpd.conf /usr/local/apache2/conf/httpd.conf
```
Build & run:
```
docker build -t my-apache .
docker run -d -p 8080:80 --name my-apache-container my-apache
```
✅ Verification
- Visit: http://localhost:8080
- You should see either the Apache default page or your custom site.
- Logs:
```
docker logs apache-server
```
