#
# Dockerfile - SSH
#
FROM     ubuntu:14.04
MAINTAINER Yongbok Kim <ruo91@yongbok.net>

# Change repository
RUN sed -i 's/archive.ubuntu.com/ftp.neowiz.com/g' /etc/apt/sources.list

# Last Package Update & Install
RUN apt-get update && apt-get install -y supervisor openssh-server aptitude net-tools iputils-ping nano

# Supervisor
RUN mkdir -p /var/log/supervisor
ADD conf/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# SSH
RUN mkdir /var/run/sshd
RUN sed -i 's/without-password/yes/g' /etc/ssh/sshd_config
RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

# Root password
RUN echo 'root:ssh' |chpasswd

# Port
EXPOSE 22

# Daemon
CMD ["/usr/bin/supervisord"]
