FROM ubuntu:latest
MAINTAINER Alex Davis <alex@alex2006hw.com>

ENV TINI_VER=0.10.0

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y -q --no-install-recommends  \
    postfix postfix-mysql postfix-pcre postgrey \
    dovecot-core dovecot-imapd dovecot-lmtpd dovecot-mysql dovecot-sieve dovecot-managesieved dovecot-pop3d \
    opendkim opendkim-tools opendmarc \
    amavisd-new amavisd-milter spamassassin clamav-daemon clamav-milter \
    libsys-syslog-perl libmail-spf-perl libhttp-message-perl altermime \
    supervisor openssl rsyslog python-pip \
    pigz pxz pbzip2 \
    wget ca-certificates dnsutils 
RUN pip install envtpl
RUN wget -q https://github.com/krallin/tini/releases/download/v$TINI_VER/tini_$TINI_VER.deb -P /tmp
RUN dpkg -i /tmp/tini_$TINI_VER.deb
RUN apt-get purge -y wget
RUN apt-get autoremove -y
RUN apt-get clean
RUN rm -rf /tmp/* /var/lib/apt/lists/* /var/cache/debconf/*-old /usr/share/doc/* /usr/share/man/*
RUN cp -r /usr/share/locale/en\@* /tmp/ && rm -rf /usr/share/locale/* && mv /tmp/en\@* /usr/share/locale/

VOLUME /var/mail /var/lib/dovecot /etc/opendkim/keys /etc/letsencrypt
EXPOSE 25 110 143 465 587 993 995 4190

COPY rootfs /
CMD ["tini","--","startup"]
