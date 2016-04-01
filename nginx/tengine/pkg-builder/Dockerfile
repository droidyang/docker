# Pull base image.
FROM debian:jessie
MAINTAINER robert.pub@gmail.com

#use mirror in china
RUN  echo "deb http://mirrors.163.com/debian/ jessie main non-free contrib"> /etc/apt/sources.list 
RUN  echo "deb http://mirrors.163.com/debian/ jessie-updates main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb http://mirrors.163.com/debian/ jessie-backports main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian/ jessie main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian/ jessie-updates main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian/ jessie-backports main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib">> /etc/apt/sources.list 

# Let the conatiner know that there is no tty
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update -qq

# Basic Requirements
RUN apt-get install -y build-essential debhelper make autoconf automake patch
RUN apt-get install -y dpkg-dev fakeroot pbuilder gnupg dh-make libssl-dev libpcre3-dev git-core

# Create Nginx user
RUN adduser --disabled-login --gecos 'Tengine' nginx 

# Install Tengine Shell
WORKDIR /home/nginx
RUN su nginx -c 'git clone https://github.com/alibaba/tengine.git'

WORKDIR /home/nginx/tengine
RUN su nginx -c 'mv packages/debian .'
RUN sed -i 's/--with-ipv6/--with-ipv6 --with-http_concat_module/g' ./debian/rules

ENV DEB_BUILD_OPTIONS nocheck 
RUN su nginx -c 'dpkg-buildpackage -rfakeroot -uc -b'

VOLUME ["/packages"]
CMD cp /home/nginx/*.deb /packages
