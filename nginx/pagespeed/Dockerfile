FROM ubuntu
MAINTAINER droid yang "yangbt.droid@gmail.com"
ENV NGX_BROTLI_STATIC_MODULE_ONLY=1
RUN apt-get update && apt-get -y upgrade
RUN apt-get -y install checkinstall \
	libpcre3-dev \
	zlib1g-dev \
	libpcre3 \
	unzip \ 
	git \
	libssl-dev
RUN apt-get clean

ADD http://nginx.org/download/nginx-1.9.12.tar.gz /root/build/
WORKDIR /root/build
RUN tar -xf nginx-1.9.12.tar.gz

ADD https://github.com/pagespeed/ngx_pagespeed/archive/release-1.10.33.6-beta.zip /root/build/
WORKDIR /root/build
RUN unzip release-1.10.33.6-beta.zip
WORKDIR /root/build/ngx_pagespeed-release-1.10.33.6-beta 
ADD https://dl.google.com/dl/page-speed/psol/1.10.33.6.tar.gz /root/build/ngx_pagespeed-release-1.10.33.6-beta/
WORKDIR /root/build/ngx_pagespeed-release-1.10.33.6-beta
RUN tar -xvzf 1.10.33.6.tar.gz

WORKDIR /root/build/ngx_brotli
RUN git clone https://github.com/google/ngx_brotli.git /root/build/ngx_brotli/

WORKDIR /root/build/nginx-upstream-fair
RUN git clone https://github.com/gnosek/nginx-upstream-fair.git /root/build/nginx-upstream-fair/


ADD ./resource/configure.sh /root/build/nginx-1.9.12/
WORKDIR /root/build/nginx-1.9.12
RUN chmod a+x configure.sh
RUN ./configure.sh && make -j4
RUN echo "droidyang: Nginx 1.9.12" > description-pak && \
	checkinstall --strip --exclude /etc/nginx/* -Dy --install=no --nodoc make -i install

CMD ["/bin/bash"]