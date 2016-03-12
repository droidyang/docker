Step 1: compile nginx with pagespeed support in docker.
  git clone https://github.com/droidyang/docker.git
  cd docker/nginx/pagespeed/pkg-builder
  sudo docker build -t nginx_ps_pkg  .
  cd ..
  ls ./packages
  
step 2: build the nginx+pagespeed docker.

 sudo docker run -it -v `pwd`/packages:/packages nginx_ps_pkg
 sudo docker build -t droid_nginx_pagespeed:1.9.12 .  
 sudo docker run -d --name nginx_pagespeed -p 80:80 -p 443:443 droid_nginx_pagespeed:1.9.12
 wget http://localhost
