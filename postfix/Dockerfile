FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
      postfix postfix-mysql postfix-pgsql postfix-ldap \
      postfix-pcre rsync

RUN mkdir /opt/postfix.default && \
    rm -rf /etc/postfix/* && \
    mkdir /etc/s6/postfix && \
    ln -s /bin/true /etc/s6/postfix/finish

ADD postfix.setup /etc/s6/postfix/setup
ADD postfix.run /etc/s6/postfix/run
COPY conf/postfix /opt/postfix.default
