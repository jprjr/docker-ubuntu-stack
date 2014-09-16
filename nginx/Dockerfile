FROM jprjr/ubuntu-base:14.04
MAINTAINER John Regan <john@jrjrtech.com>

ADD add-ldap-module.patch /tmp/add-ldap-module.patch

RUN export DEBIAN_FRONTEND=noninteractive && \
    echo "deb http://nginx.org/packages/ubuntu/ trusty nginx" > /etc/apt/sources.list.d/nginx.list && \
    echo "deb-src http://nginx.org/packages/ubuntu/ trusty nginx" >> /etc/apt/sources.list.d/nginx.list && \
    apt-key adv --fetch-keys "http://nginx.org/keys/nginx_signing.key" &&  \
    apt-get update && \
    apt-get -y install wget git nginx libldap2-dev && \
    apt-get -y build-dep nginx && \
    apt-get -y remove nginx && \
    cd /tmp && apt-get source nginx && \
    mkdir -p /tmp/mods && cd /tmp/mods && \
    git clone https://github.com/kvspb/nginx-auth-ldap.git && \
    cd /tmp/mods/nginx-auth-ldap && wget -O fix-compile.patch https://github.com/huangsam/nginx-auth-ldap/commit/eaae4dac6f965c620102b9f95db3c006b11fd431.patch && \
    patch -i fix-compile.patch && \
    cd /tmp/nginx-* && \
    patch -p1 -i /tmp/add-ldap-module.patch && \
    debian/rules binary && \
    cd / && dpkg -i /tmp/nginx_*.deb && \
    rm -rf /etc/nginx/conf.d/* && \
    rm -rf /srv/www/* && \
    apt-mark auto autotools-dev debhelper  dh-systemd  dpkg-dev  libexpat-dev \
    libgd2-dev libgd2-noxpm-dev libgeoip-dev liblua5.1-dev libmhash-dev \
    libpam0g-dev libpcre3-dev libperl-dev libssl-dev libxslt1-dev po-debconf\
     zlib1g-dev debhelper libssl-dev libpcre3-dev zlib1g-dev && \
    apt-mark manual sudo && \
    apt-get remove -y build-essential libldap2-dev git && \
    apt-get autoremove -y && \
    userdel nginx

COPY root /

RUN chmod 0644 /etc/logrotate.jprjr.d/nginx

EXPOSE 80
EXPOSE 443
