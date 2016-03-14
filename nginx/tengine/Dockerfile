FROM debian:jessie
MAINTAINER yangbt.droid@gmail.com

ENV TENGINE_VERSION 2.1.0-1

#use mirror in china
RUN  echo "deb http://mirrors.163.com/debian/ jessie main non-free contrib"> /etc/apt/sources.list 
RUN  echo "deb http://mirrors.163.com/debian/ jessie-updates main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb http://mirrors.163.com/debian/ jessie-backports main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian/ jessie main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian/ jessie-updates main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian/ jessie-backports main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib">> /etc/apt/sources.list 

ADD packages/tengine_${TENGINE_VERSION}_amd64.deb /tmp/tengine.deb

RUN apt-get update && \
    apt-get install -y ca-certificates && \
    dpkg -i /tmp/tengine.deb && \
    rm -rf /var/lib/apt/lists/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
