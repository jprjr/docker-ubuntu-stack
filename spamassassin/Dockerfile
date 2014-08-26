FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
    spampd 

RUN mkdir /etc/s6/spampd && \
    ln -s /bin/true /etc/s6/spampd/finish

ADD spampd.run /etc/s6/spampd/run

ADD conf/spampd.conf /etc/spampd.conf

EXPOSE 10025
