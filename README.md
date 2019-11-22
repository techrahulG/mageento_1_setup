# magento_1_setup
Dockerfile and Docker-compose.yml file for magento 1
# Dockerfile 

#Dockerfile start with FROM Tag which is allow to pull image

FROM centos:7

#RUN tag is use to run command inside the image you have pulled

RUN yum install -y net-tools

RUN yum update -y

RUN yum install -y  https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

RUN yum install -y  http://rpms.remirepo.net/enterprise/remi-release-7.rpm

RUN rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

RUN curl -sL https://rpm.nodesource.com/setup_8.x | bash -

RUN yum-config-manager --enable remi-php56

RUN yum install -y \
       php \
       php-mcrypt \
       php-cli \
       php-gd \
       php-curl \
       php-mysql \
       php-ldap\
       php-zip \
       php-fileinfo \
       php-fpm \
       php-devel \
       php-mbstring \
       php-mcrypt \
       php-soap \
       php-apc \
       php-common \
       php-pear \
    yum install -y git \
    yum install -y initscripts \
    yum install -y wget \
    yum install epel-release \
    yum install -y nginx \
    yum install -y firewalld \
    yum install -y mariadb-server \
    yum install -y nodejs
#here it is updating cgi.fix_pathinfo=1 to cgi.fix_pathinfo=0 and also uncommenting it.
RUN   sed -i "s|cgi.fix_pathinfo=1|cgi.fix_pathinfo=0|g" /etc/php.ini
RUN sed -i '/cgi.fix_pathinfo/s/^;//g' /etc/php.ini

#you have to copy your default.conf file inside containers /etc/nginx/conf.d
COPY  default.conf  /etc/nginx/conf.d

#when you check for services of nginx and php-fpm youll get D-Bus error to avoid that this is the solution
ENV container=docker
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;

#service will start automatically but we need to enable it.
RUN systemctl enable nginx.service
RUN systemctl enable php-fpm.service

#To expose port 80 for cotainer
expose 80

#For  run subsequent commands of services
CMD /usr/sbin/init


# docker-compose.yml
#version is very important part of this file which is depend on docker version.
version: "3.3"

#inside services you can define project title.
services:
  magento:
  
#here Dockerfile is getting called   
build: .  

#after building Dockerfile this will tag image with name and container
image: 'rahuldock16/magento'       
#set the container name 
container_name: 'magento_1'        
#allow you to make changes in prodution environment also
 environment:
      PRODUCTION: 'true'    
      
      
      
              
