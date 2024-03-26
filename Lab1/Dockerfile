FROM ubuntu:18.04
MAINTAINER michel@gmail.com
RUN apt-get update
RUN apt-get install -y nginx 
EXPOSE 80
ADD . /var/www/html/
ENTRYPOINT ["/usr/sbin/nginx", "-g", "daemon off;"]
