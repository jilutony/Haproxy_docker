
 I have used HAproxy load balancer inside of  Docker containers.
 
 Once installed the Docker, use the following command to create a new bridge network in Docker:
 
 sudo docker network create --driver=bridge mynetwork
 
 Then use the docker run command to create and run two instances of the web application one using httpd and nginx
 
 docker run --name apachewebsite --network=mynetwork -v /home/ec2-user/consultancy-website-template:/usr/local/apache2/htdocs -p 8087:80 -itd httpd:latest
 docker run --name nginxtest  --network=mynetwork -v /home/ec2-user/html:/usr/share/nginx/html -p 8082:80 -tid nginx:latest
 
 [i have bind mount the website template using -v to the document root of the webserver]
 
 Then Create a file named haproxy.cfg add the neccessarychanges
 
 Next, create and run an HAProxy container and map its port 80 to host's 80 port by including the -p argument. Also map port 8404 for the HAProxy Stats page:

$ sudo docker run -d \
   --name haproxy \
   --net mynetwork \
   -v $(pwd):/usr/local/etc/haproxy:ro \
   -p 80:80 \
   -p 8404:8404 \
   haproxytech/haproxy-alpine:2.4
   
   Reload the haproxy configuration after done anychanges to that file.
   
   sudo docker kill -s HUP haproxy
   
   
