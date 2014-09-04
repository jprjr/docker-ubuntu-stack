FROM ubuntu:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get install -y openssl ca-certificates \
     rsyslog rsyslog-relp cron curl rsync logrotate gettext-base && \
    apt-mark manual openssl ca-certificates \
     rsyslog rsyslog-relp cron curl rsync logrotate gettext-base && \
    apt-get clean 

COPY root /
RUN mkdir -p /etc/logstash/ssl && \ 
    chmod 0644 /etc/logrotate.conf

ENTRYPOINT ["/usr/bin/s6-svscan","/etc/s6"]
CMD []
