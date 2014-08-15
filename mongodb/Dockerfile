FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN export DEBIAN_FRONTEND=noninteractive && \
    echo "deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen" > /etc/apt/sources.list.d/mongodb.list && \
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10 && \
    apt-get update && \
    apt-get -y install mongodb-org

#ADD nginx.conf /etc/nginx/nginx.conf
#ADD nginx.run /etc/s6/nginx/run
#ADD default.conf /etc/nginx/conf.d/default.conf
#ADD index.html /srv/www/index.html

#VOLUME ["/etc/nginx"]
#VOLUME ["/srv/www"]

#EXPOSE 80
#EXPOSE 443
