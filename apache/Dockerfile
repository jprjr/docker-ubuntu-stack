FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get -y install apache2 && \
    mkdir -p /var/lock/apache2

COPY root /

EXPOSE 80
EXPOSE 443
