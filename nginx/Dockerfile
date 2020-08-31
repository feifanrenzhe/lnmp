FROM centos:latest

ARG SYSTEM_TIMEZONE=Asia/ShangHai
RUN ln -snf /usr/share/zoneinfo/$SYSTEM_TIMEZONE /etc/localtime && \
    echo $SYSTEM_TIMEZONE > /etc/timezone

# Add repository and keys
RUN yum clean all; yum -y update; yum -y install epel-release

# Install nginx-1.16.0
COPY .docker/nginx.repo /etc/yum.repos.d/nginx.repo
RUN yum -y install --enablerepo=nginx nginx

# Copy nginx conf
COPY .docker/default.conf /etc/nginx/conf.d/default.conf

# Enable nginx and start
CMD ["nginx", "-g", "daemon off;"]

# Install Package
RUN yum -y install git make autoconf curl wget perl curl-devel expat-devel gettext-devel perl-devel perl-core \
    gcc gcc-c++ glibc-headers openssl-devel readline readline-devel zlib zlib-devel sqlite-devel bzip2

# Download ruby and bundle
#RUN git config --global http.postBuffer 1048576000 ; \
#    git clone https://github.com/rbenv/rbenv.git /usr/local/rbenv ; \
#    git clone https://github.com/rbenv/ruby-build.git /usr/local/rbenv/plugins/ruby-build

# setting for rbenv
#RUN echo 'export RBENV_ROOT="/usr/local/rbenv"' >> /etc/profile.d/rbenv.sh
#RUN echo 'export PATH="${RBENV_ROOT}/bin:${PATH}"' >> /etc/profile.d/rbenv.sh
#RUN echo 'eval "$(rbenv init --no-rehash -)"' >> /etc/profile.d/rbenv.sh
#RUN source /etc/profile.d/rbenv.sh; rbenv install 2.6.3; rbenv global 2.6.3

# Install OpenSSL
ADD .docker/openssl-1.1.1.tar.gz /usr/local/src/
WORKDIR /usr/local/src/openssl-1.1.1/
RUN ./config --prefix=/usr/local/openssl-1.1.1 shared zlib ; make depend; make; make test; make install
COPY .docker/openssl-1.1.1.conf /etc/ld.so.conf.d/openssl-1.1.1.conf
RUN ldconfig

# Install postfix
#RUN yum -y install postfix rsyslog; \ 
#    systemctl enable postfix.service

# Install git 2.21

ADD .docker/git-2.21.0.tar.xz /
WORKDIR /git-2.21.0
RUN make configure ; ./configure --prefix=/usr/local ; make ; make install

WORKDIR /var/www/

EXPOSE 80 443
