FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get -y install apache2 && \
    mkdir -p /var/lock/apache2
#    rm -rf /etc/nginx/conf.d/* && \
#    rm -rf /srv/www/* && \
#    mkdir /etc/s6/nginx && \
#    ln -s /bin/true /etc/s6/nginx/finish && \
#    userdel nginx

#ADD nginx.conf /etc/nginx/nginx.conf
#ADD nginx.run /etc/s6/nginx/run
#ADD default.conf /etc/nginx/conf.d/default.conf
#ADD index.html /srv/www/index.html

COPY root /

EXPOSE 80
EXPOSE 443
