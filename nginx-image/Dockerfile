# Base image
FROM ubuntu:latest

# LABEL para a metadata
LABEL maintainer="filipecalau@gmail.com" 

# Install nginx 
RUN apt-get update -y && apt-get install -y nginx

# Copiar os arquivos de configurações e content
COPY files/default /etc/nginx/sites-available/default
COPY files/index.html /usr/share/nginx/html/index.html

# Expor o container na porta 80
EXPOSE 80

# Start ngnix 
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]


