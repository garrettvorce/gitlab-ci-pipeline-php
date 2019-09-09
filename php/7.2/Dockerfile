FROM php:7.2

ARG BUILD_DATE
ARG VCS_REF

LABEL maintainer="Eduardo Bizarro <eduardo@zendev.com.br>" \
  PHP="7.2" \
  NODE="12.10.0" \
  org.label-schema.name="edbizarro/gitlab-ci-pipeline-php" \
  org.label-schema.description=":coffee: Docker images for build and test PHP applications with Gitlab CI (or any other CI plataform!)" \
  org.label-schema.build-date=$BUILD_DATE \
  org.label-schema.schema-version="1.0" \
  org.label-schema.vcs-url="https://github.com/edbizarro/gitlab-ci-pipeline-php" \
  org.label-schema.vcs-ref=$VCS_REF

# Set correct environment variables
ENV IMAGE_USER=php
ENV HOME=/home/$IMAGE_USER
ENV COMPOSER_HOME=$HOME/.composer
ENV PATH=$HOME/.yarn/bin:$PATH
ENV GOSS_VERSION="0.3.6"
ENV PHP_VERSION=7.2

USER root

WORKDIR /tmp

# COPY INSTALL SCRIPTS
COPY --from=composer:1 /usr/bin/composer /usr/bin/composer

# Install
RUN apt-get install -y software-properties-common \
 && apt-add-repository ppa:ondrej/php
RUN apt-get update
RUN add-apt-repository ppa:ondrej/php
RUN apt-get -y install unzip zip nginx php7.2 php7.2-mysql php7.2-fpm php7.2-mbstring php7.2-xml php7.2-curl
RUN adduser --disabled-password --gecos "" $IMAGE_USER
RUN echo "PATH=$(yarn global bin):$PATH" >> /root/.profile 
RUN echo "PATH=$(yarn global bin):$PATH" >> /root/.bashrc
RUN echo "$IMAGE_USER  ALL = ( ALL ) NOPASSWD: ALL" >> /etc/sudoers
RUN mkdir -p /var/www/html
RUN chown -R $IMAGE_USER:$IMAGE_USER /var/www $HOME
RUN composer global require "hirak/prestissimo:^0.3"
RUN rm -rf ~/.composer/cache/*
RUN chown -R $IMAGE_USER:$IMAGE_USER $COMPOSER_HOME
RUN curl -fsSL https://goss.rocks/install | GOSS_VER=v${GOSS_VERSION} sh
RUN apt-get update
RUN apt-get install -y software-properties-common
RUN apt-get -qq install sshpass -y

USER $IMAGE_USER

WORKDIR /var/www/html
