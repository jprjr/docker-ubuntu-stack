FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN export DEBIAN_FRONTEND=noninteractive && \
    echo "deb http://nginx.org/packages/ubuntu/ trusty nginx" >> /etc/apt/sources.list.d/nginx.list && \
    apt-key adv --fetch-keys "http://nginx.org/keys/nginx_signing.key" &&  \
    apt-get update && \
    apt-get -y install nginx && \
    rm -rf /etc/nginx/conf.d/* && \
    rm -rf /srv/www/* && \
    mkdir /etc/s6/nginx && \
    ln -s /bin/true /etc/s6/nginx/finish && \
    userdel nginx

ADD nginx.conf /etc/nginx/nginx.conf
ADD nginx.run /etc/s6/nginx/run
ADD nginx.setup /etc/s6/nginx/setup
ADD nginx.logrotate /etc/logrotate.jprjr.d/nginx
ADD default.conf /etc/nginx/conf.d/default.conf
ADD index.html /srv/www/index.html
ADD logstash-forwarder.conf /etc/logstash-forwarder.conf

EXPOSE 80
EXPOSE 443
