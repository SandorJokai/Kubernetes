FROM debian:10

LABEL maintainer Sandor Jokai <shanxman1986@gmail.com>

RUN apt update && apt install -y no-install-recommends apache2 libapache2-mod-php7.3 php7.3-gd \
php-getid3 php7.3-mysql php7.3-cli php7.3-json php7.3-opcache php7.3-readline apparmor-profiles \
libapache2-mod-apparmor ca-certificates ssl-cert gnutls-bin git && apt-get clean && rm -rf /var/lib/apt/\
lists/*

ENV APACHE_CONFDIR /etc/apache2

ENV APACHE_ENVVARS $APACHE_CONFDIR/envvars

#Probably this is not the best idea despite it's not presents any danger to my other github repo (when I created
#the Personal access tokens, I only permitted to accept the wordpress repository with only read permisson),
#I could do that only with this method.
ENV GITKEY 11ade8b86d0dd41d24d11d536e3642ad5c86e68b

RUN a2enmod rewrite php7.3 && a2dismod mpm_event && a2enmod mpm_prefork && mkdir -p /var/lock/\
apache2 /var/run/apache2

RUN git clone -b prod https://${GITKEY}@github.com/SandorJokai/wordpress.git /var/www/html && apt-get purge \
-y git && apt-get purge --autoremove -y && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html

EXPOSE 80 443

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
