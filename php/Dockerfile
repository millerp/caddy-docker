FROM alpine:3.2
MAINTAINER Miller P. Magalhães <miller@mpdev.com.br>

LABEL caddy_version="0.8.3" architecture="amd64"

RUN apk add --update openssh-client git tar php-fpm

# essential php libs
RUN apk add php-curl php-gd php-zip php-iconv php-sqlite3 php-mysql php-mysqli php-json php-pdo php-openssl php-ctype

# allow environment variable access.
RUN echo "clear_env = no" >> /etc/php/php-fpm.conf

# Redis Server
RUN apk add redis
RUN redis-server --daemonize yes

RUN curl --silent --show-error --fail --location \
      --header "Accept: application/tar+gzip, application/x-gzip, application/octet-stream" -o - \
      "https://caddyserver.com/download/build?os=linux&arch=amd64&features=git" \
    | tar --no-same-owner -C /usr/bin/ -xz caddy \
 && chmod 0755 /usr/bin/caddy \
 && /usr/bin/caddy -version

EXPOSE 80 443 2015
VOLUME /srv
WORKDIR /srv

ADD Caddyfile /etc/Caddyfile
ADD index.php /srv/index.php

ENTRYPOINT ["/usr/bin/caddy"]
CMD ["--conf", "/etc/Caddyfile"]
