Step 1: compile nginx with pagespeed support in compiler docker. </br>
  git clone https://github.com/droidyang/docker.git </br>
  cd docker/nginx/pagespeed/pkg-builder </br>
  sudo docker build -t nginx_ps_pkg  .  </br>
  cd ..  </br>
  ls ./packages  </br>
  
step 2: build the nginx+pagespeed docker.  </br>

 sudo docker run -it -v `pwd`/packages:/packages nginx_ps_pkg   </br>
 sudo docker build -t droid_nginx_pagespeed:1.9.12 .     </br>
 sudo docker run -d --name nginx_pagespeed -p 80:80 -p 443:443 droid_nginx_pagespeed:1.9.12   </br>
 wget http://localhost  </br>
