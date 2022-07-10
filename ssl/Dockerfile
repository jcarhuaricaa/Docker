FROM  centos:7

RUN \ 
  yum -y install \
    httpd \
    php \
    php-mysql \
    mod_ssl \
    openssl

COPY html-ssl /var/www/html
RUN echo "<?php phpinfo(); ?>" >/var/www/html/test.php

COPY docker.crt /docker.crt

COPY docker.key /docker.key

COPY ssl.conf /etc/httpd/conf.d/default.conf

CMD apachectl -DFOREGROUND
