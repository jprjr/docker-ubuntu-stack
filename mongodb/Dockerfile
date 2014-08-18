FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN export DEBIAN_FRONTEND=noninteractive && \
    echo "deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen" > /etc/apt/sources.list.d/mongodb.list && \
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10 && \
    apt-get update && \
    apt-get -y install mongodb-org

RUN mkdir -p /etc/s6/mongodb && \
    ln -s /bin/true /etc/s6/mongodb/finish && \
    mkdir -p /var/lib/mongodb && \
    touch /var/lib/mongodb/.dummy && \
    chown -R mongodb /var/lib/mongodb

ADD mongodb.run /etc/s6/mongodb/run
ADD conf/mongod.conf /etc/mongod.conf

VOLUME ["/var/lib/mongodb"]

EXPOSE 27017
