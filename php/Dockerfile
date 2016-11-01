#
# Apache + PHP
#
# 2015-06-22
#   CentOS 6.6 + epel,remi,rpmforge
#   Apache 2.2.15
#   PHP 5.5.26

FROM centos:6
MAINTAINER shtoma

# update yum
RUN yum update -y && \
    yum clean all

# epel repo
RUN yum install -y http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm && \
    yum clean all
RUN sed -i -e "s/enabled *= *1/enabled=0/g" /etc/yum.repos.d/epel.repo

# remi repo
RUN yum install -y http://rpms.famillecollet.com/enterprise/remi-release-6.rpm && \
    yum clean all
RUN sed -i -e "s/enabled *= *1/enabled=0/g" /etc/yum.repos.d/remi.repo

# rpmforge repo
#RUN yum install -y http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm && \
RUN yum install -y http://ftp.tu-chemnitz.de/pub/linux/dag/redhat/el6/en/x86_64/rpmforge/RPMS/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm && \
    yum clean all
RUN sed -i -e "s/enabled *= *1/enabled=0/g" /etc/yum.repos.d/rpmforge.repo

# httpd
RUN yum install -y httpd httpd-tools && \
    yum clean all

# libmcrypt, s3cmd
RUN yum install --enablerepo=epel -y libmcrypt s3cmd libwebp && \
    yum clean all

# gd-last (for php-gd)
RUN yum install --enablerepo=remi -y gd-last && \
    yum clean all

# php-pecl-memcached
RUN yum install --enablerepo=remi,remi-php55 -y php-pecl-memcached && \
    yum clean all

# php
RUN yum install --enablerepo=remi-php55 -y php php-devel php-gd php-mbstring php-mcrypt php-mysqlnd php-pear php-xml php-opcache && \
    yum clean all

# modify /etc/php.ini
RUN sed -i -e "s/;date.timezone *=.*$/date.timezone = Asia\/Tokyo/" /etc/php.ini

ENV WEBAPP_ROOT /webapp

ADD ./httpd.conf /etc/httpd/conf/httpd.conf
ADD ./index.html /webapp/public/index.html
ADD ./phpinfo.php /webapp/public/phpinfo.php

EXPOSE 80

CMD ["/usr/sbin/httpd","-D","FOREGROUND"]
