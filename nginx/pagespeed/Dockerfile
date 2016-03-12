FROM debian:jessie

RUN  echo "deb http://mirrors.163.com/debian/ jessie main non-free contrib"> /etc/apt/sources.list 
RUN  echo "deb http://mirrors.163.com/debian/ jessie-updates main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb http://mirrors.163.com/debian/ jessie-backports main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian/ jessie main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian/ jessie-updates main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian/ jessie-backports main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib">> /etc/apt/sources.list 
RUN  echo "deb-src http://mirrors.163.com/debian-security/ jessie/updates main non-free contrib">> /etc/apt/sources.list 

# Install build tools for nginx
RUN apt-get update && \ 
   apt-get install -y wget ca-certificates build-essential zlib1g-dev libpcre3 libpcre3-dev unzip debhelper libssl-dev 

WORKDIR /root/build
RUN wget http://nginx.org/download/nginx-1.9.12.tar.gz
RUN wget https://github.com/pagespeed/ngx_pagespeed/archive/release-1.10.33.6-beta.zip
RUN tar -xf nginx-1.9.12.tar.gz
RUN unzip release-1.10.33.6-beta.zip

WORKDIR /root/build/ngx_pagespeed-release-1.10.33.6-beta 
RUN wget  https://dl.google.com/dl/page-speed/psol/1.10.33.6.tar.gz 
RUN tar -xvzf 1.10.33.6.tar.gz

ADD ./resource/configure.sh /root/build/nginx-1.9.12/
WORKDIR /root/build/nginx-1.9.12
RUN chmod a+x configure.sh 
RUN ./configure.sh && make -j4  && make install  

# forward request and error logs to docker log collector 
RUN ln -sf /dev/stdout /var/log/nginx/access.log \ 
	&& ln -sf /dev/stderr /var/log/nginx/error.log 

RUN apt-get clean all
RUN rm -rf /var/lib/apt/lists/*
RUN rm -Rf /root/build/* 

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
