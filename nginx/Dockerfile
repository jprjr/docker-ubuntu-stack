FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN export DEBIAN_FRONTEND=noninteractive && \
    echo "deb http://nginx.org/packages/ubuntu/ trusty nginx" >> /etc/apt/sources.list.d/nginx.list && \
    apt-key adv --fetch-keys "http://nginx.org/keys/nginx_signing.key" &&  \
    apt-get update && \
    apt-get -y install nginx && \
    rm -rf /etc/nginx/conf.d/* && \
    rm -rf /srv/www/* && \
    userdel nginx

ADD nginx.conf /etc/nginx/nginx.conf
ADD default.conf /etc/nginx/conf.d/default.conf
ADD index.html /srv/www/index.html

VOLUME ["/etc/nginx"]
VOLUME ["/srv/www"]

EXPOSE 80
EXPOSE 443
