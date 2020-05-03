FROM debian:10

LABEL maintainer Sandor Jokai <shanxman1986@gmail.com>

RUN apt update && apt install -y no-install-recommends apache2 libapache2-mod-php7.3 php7.3-gd \
php-getid3 php7.3-mysql php7.3-cli php7.3-json php7.3-opcache php7.3-readline apparmor-profiles \
libapache2-mod-apparmor ca-certificates ssl-cert gnutls-bin && apt-get clean && rm -rf /var/lib/apt/\
lists/*

ENV APACHE_CONFDIR /etc/apache2

ENV APACHE_ENVVARS $APACHE_CONFDIR/envvars

RUN a2enmod rewrite php7.3 && a2dismod mpm_event && a2enmod mpm_prefork && mkdir -p /var/lock/\
apache2 /var/run/apache2

WORKDIR /var/www

EXPOSE 80 443

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
