#
# Dockerfile - OpenResty
#
FROM     ubuntu:14.04
MAINTAINER Yongbok Kim <ruo91@yongbok.net>

# Change repository
RUN sed -i 's/archive.ubuntu.com/ftp.neowiz.com/g' /etc/apt/sources.list

# Last Package Update & Install
RUN apt-get update && apt-get install -y curl build-essential libreadline6-dev \
 ncurses-dev libpcre++-dev libssl-dev libgeoip-dev libxml2-dev libxslt-dev libgd2-xpm-dev libperl-dev supervisor openssh-server aptitude net-tools iputils-ping nano

# OpenResty
ENV SRC_DIR /opt
ENV OPENRESTY_VERSION 1.5.12.1
ENV OPENRESTY_PREFIX /etc/openresty
RUN cd $SRC_DIR && curl -LO http://openresty.org/download/ngx_openresty-$OPENRESTY_VERSION.tar.gz \
 && tar xzf ngx_openresty-$OPENRESTY_VERSION.tar.gz && cd ngx_openresty-$OPENRESTY_VERSION \
 && ./configure --prefix=$OPENRESTY_PREFIX \
 --user=www-data \
 --group=www-data \
 --sbin-path=/usr/sbin \
 --pid-path=/tmp/openresty.pid \
 --lock-path=/tmp/openresty.lock \
 --with-file-aio \
 --with-ipv6 \
 --with-http_realip_module \
 --with-http_addition_module \
 --with-http_xslt_module \
 --with-http_image_filter_module \
 --with-http_geoip_module \
 --with-http_sub_module \
 --with-http_dav_module \
 --with-http_flv_module \
 --with-http_iconv_module \
 --with-http_gzip_static_module \
 --with-http_random_index_module \
 --with-http_secure_link_module \
 --with-http_degradation_module \
 --with-http_stub_status_module \
 --with-http_perl_module \
 --with-pcre --with-pcre-jit --with-md5-asm --with-sha1-asm \
 --http-client-body-temp-path=/tmp/openresty_client_body_temp \
 --http-proxy-temp-path=/tmp/openresty_proxy_temp \
 --http-fastcgi-temp-path=/tmp/openresty_fastcgi_temp \
 && make && make install && rm -rf ngx_openresty-$OPENRESTY_VERSION*

# Add in the conf directory
ADD conf/nginx.conf $OPENRESTY_PREFIX/nginx/conf/nginx.conf
ADD conf/01_vhosts_default.conf $OPENRESTY_PREFIX/nginx/conf/vhosts/01_vhosts_default.conf
ADD conf/robots.txt $OPENRESTY_PREFIX/nginx/conf/vhosts/robots.txt

# Unset ENV
RUN unset SRC_DIR OPENRESTY_VERSION OPENRESTY_PREFIX

# Supervisor
RUN mkdir -p /var/log/supervisor
ADD conf/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# SSH
RUN mkdir /var/run/sshd
RUN sed -i 's/without-password/yes/g' /etc/ssh/sshd_config
RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

# Root password
RUN echo 'root:openresty' |chpasswd

# Port
EXPOSE 22 80

# Daemon
CMD ["/usr/bin/supervisord"]
