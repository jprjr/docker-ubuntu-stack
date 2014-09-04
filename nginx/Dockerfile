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

COPY root /

RUN chmod 0644 /etc/logrotate.jprjr.d/nginx

EXPOSE 80
EXPOSE 443
