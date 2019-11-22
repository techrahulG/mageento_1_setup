# mageento_1_setup
Dockerfile and Docker-compose.yml file for magento 1


Dockerfile 
# Dockerfile start with FROM Tag which is allow to pull image
FROM centos:7
# RUN tag is use to run command inside the image you have pulled
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
# here it is updating cgi.fix_pathinfo=1 to cgi.fix_pathinfo=0 and also uncommenting it.
RUN   sed -i "s|cgi.fix_pathinfo=1|cgi.fix_pathinfo=0|g" /etc/php.ini
RUN sed -i '/cgi.fix_pathinfo/s/^;//g' /etc/php.ini

# you have to copy your default.conf file inside containers /etc/nginx/conf.d
COPY  default.conf  /etc/nginx/conf.d

