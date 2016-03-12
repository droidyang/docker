FROM debian:jessie
MAINTAINER yangbt.droid@gmail.com

ENV NGINX_VERSION 1.9.12 
ENV NPS_VERSION 1.10.33.6 
ENV SETUP_DIR /var/cache/nginx

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
   apt-get install -y wget ca-certificates build-essential zlib1g-dev libpcre3 libpcre3-dev unzip debhelper libssl-dev libgeoip-dev 

RUN mkdir -p ${SETUP_DIR}
WORKDIR ${SETUP_DIR}
RUN wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz
RUN wget https://github.com/pagespeed/ngx_pagespeed/archive/release-${NPS_VERSION}-beta.zip
RUN tar -xf nginx-${NGINX_VERSION}.tar.gz
RUN unzip release-1.10.33.6-beta.zip

WORKDIR ${SETUP_DIR}/ngx_pagespeed-release-${NPS_VERSION}-beta 
RUN wget  https://dl.google.com/dl/page-speed/psol/${NPS_VERSION}.tar.gz 
RUN tar -xvzf ${NPS_VERSION}.tar.gz


WORKDIR ${SETUP_DIR}/nginx-${NGINX_VERSION}.

RUN ./configure \
	--prefix=/etc/nginx \
	--sbin-path=/usr/sbin/nginx \
	--conf-path=/etc/nginx/nginx.conf \
	--with-cc-opt='-g -O2 -fPIE -fstack-protector --param=ssp-buffer-size=4 -Wformat -Werror=format-security -D_FORTIFY_SOURCE=2' --with-ld-opt='-Wl,-Bsymbolic-functions -fPIE -pie -Wl,-z,relro -Wl,-z,now' \
	--http-log-path=/var/log/nginx/access.log  \
	--error-log-path=/var/log/nginx/error.log \
	--pid-path=/var/run/nginx.pid \ 
 	--lock-path=/var/run/nginx.lock \
    --http-fastcgi-temp-path=/var/lib/nginx/fastcgi \
	--with-http_addition_module \
	--with-http_auth_request_module \
	--with-http_dav_module \
	--with-http_degradation_module \
	--with-http_flv_module \
	--with-http_gzip_static_module \
	--with-http_gunzip_module \
	--with-http_geoip_module \
	--with-http_image_filter_module \
	--with-http_mp4_module \
	--with-http_realip_module \
	--with-http_random_index_module \
	--with-http_secure_link_module \
	--with-http_stub_status_module \
	--with-http_stub_status_module \
	--with-http_sub_module \
	--with-http_ssl_module \
	--with-http_slice_module \
	--with-http_v2_module \
	--with-http_xslt_module \
	--with-file-aio \
	--with-ipv6 \
	--with-mail \ 
    --with-mail_ssl_module \ 
	--with-stream \
	--with-stream_ssl_module \
	--with-threads \
	--add-module=../ngx_pagespeed-release-${NPS_VERSION}-beta
RUN make -j4  && make install  

# forward request and error logs to docker log collector 
RUN ln -sf /dev/stdout /var/log/nginx/access.log \ 
	&& ln -sf /dev/stderr /var/log/nginx/error.log 

RUN apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false -o APT::AutoRemove::SuggestsImportant=false \ 
     wget \ 
     libpcre3-dev \ 
     zlib1g-dev \ 
     openssl \ 
     build-essential \ 
     zlib1g-dev \ 
     libssl-dev \ 
  	 libgeoip-dev


RUN apt-get clean all
RUN rm -rf /var/lib/apt/lists/*
RUN rm -Rf ${SETUP_DIR}/* 

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
