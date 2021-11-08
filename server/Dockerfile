FROM centos:centos8

#Variaveis de ambiente para baixar o RPM do NGINX
ENV nginxversion="1.20.1-1" \
    os="centos" \
    osversion="8" \
    elversion="8"

#Habilitaar o systemd
RUN (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == \
systemd-tmpfiles-setup.service ] || rm -f $i; done); \
rm -f /lib/systemd/system/multi-user.target.wants/*;\
rm -f /etc/systemd/system/*.wants/*;\
rm -f /lib/systemd/system/local-fs.target.wants/*; \
rm -f /lib/systemd/system/sockets.target.wants/*udev*; \
rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
rm -f /lib/systemd/system/basic.target.wants/*;\
rm -f /lib/systemd/system/anaconda.target.wants/*;
VOLUME [ "/sys/fs/cgroup" ]

#adicionando editores de texto
RUN yum update -y && \
    yum install -y nano vim

RUN yum install -y epel-release && \
    yum install -y \
    iproute \
    hostname \
    inotify-tools \
    yum-utils \
    which \
    jq \
    rsync

RUN yum install -y wget openssl sed &&\
    yum -y autoremove &&\
    wget http://nginx.org/packages/$os/$osversion/x86_64/RPMS/nginx-$nginxversion.el$elversion.ngx.x86_64.rpm &&\
    rpm -iv nginx-$nginxversion.el$elversion.ngx.x86_64.rpm &&\
    sed -i '1i\
    daemon off;\
    ' /etc/nginx/nginx.conf


# Install PHP
RUN yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
RUN yum install -y http://rpms.remirepo.net/enterprise/remi-release-8.rpm
RUN yum module disable php -y
RUN yum module enable php:remi-8.0 -y
RUN yum -y install php php-fpm php-bcmath php-cli php-common php-gd php-intl php-json php-ldap php-mbstring \
    php-mysqlnd php-pdo php-pear php-soap php-xml php-xmlrpc php-pecl-zip

#RUN sed -i 's/listen.owner = www-data/listen.owner = nginx/g' /etc/php-fpm.d/www.conf
#RUN sed -i 's/listen.group = www-data/listen.group = nginx/g' /etc/php-fpm.d/www.conf

RUN yum clean all

EXPOSE 80
EXPOSE 443

CMD /usr/sbin/php-fpm;nginx