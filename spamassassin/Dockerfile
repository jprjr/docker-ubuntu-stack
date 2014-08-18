FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    spampd cron rsyslog

RUN mkdir /etc/s6/cron && \
    mkdir /etc/s6/spampd && \
    mkdir /etc/s6/syslog && \
    ln -s /bin/true /etc/s6/cron/finish && \
    ln -s /bin/true /etc/s6/spampd/finish && \
    ln -s /bin/true /etc/s6/syslog/finish && \
    rm -rf /etc/rsyslog.d && \
    rm -f /etc/rsyslog.conf

ADD cron.run /etc/s6/cron/run
ADD syslog.setup /etc/s6/syslog/setup
ADD syslog.run /etc/s6/syslog/run
ADD spampd.run /etc/s6/spampd/run

COPY conf/rsyslog /etc/
ADD conf/spampd.conf /etc/spampd.conf

EXPOSE 10025
