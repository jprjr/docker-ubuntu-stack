FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y \
      dovecot-core dovecot-imapd dovecot-lmtpd \
      dovecot-sieve dovecot-managesieved dovecot-mysql dovecot-pgsql \
      dovecot-sqlite dovecot-antispam dovecot-ldap dovecot-solr \
      rsync mysql-client

RUN mkdir /opt/dovecot.default && \
    rm -rf /etc/dovecot/* && \
    mkdir /etc/s6/dovecot && \
    ln -s /bin/true /etc/s6/dovecot/finish && \
    mkdir -p /srv/mail/sieve && \
    mkdir -p /srv/mail/mailboxes

ADD dovecot.run /etc/s6/dovecot/run
ADD dovecot.setup /etc/s6/dovecot/setup
ADD hash_password /opt/hash_password
COPY conf /opt/dovecot.default

EXPOSE 143
EXPOSE 993
EXPOSE 12345
EXPOSE 10026
