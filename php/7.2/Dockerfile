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
RUN apt-get install software-properties-common \ 
  && apt-get update \
  && add-apt-repository ppa:ondrej/php \
  && apt-get -y install unzip zip nginx php7.2 php7.2-mysql php7.2-fpm php7.2-mbstring php7.2-xml php7.2-curl \
  adduser --disabled-password --gecos "" $IMAGE_USER && \
  echo "PATH=$(yarn global bin):$PATH" >> /root/.profile && \
  echo "PATH=$(yarn global bin):$PATH" >> /root/.bashrc && \
  echo "$IMAGE_USER  ALL = ( ALL ) NOPASSWD: ALL" >> /etc/sudoers && \
  mkdir -p /var/www/html && \
  chown -R $IMAGE_USER:$IMAGE_USER /var/www $HOME \
  && composer global require "hirak/prestissimo:^0.3"  \
  && rm -rf ~/.composer/cache/* \
  && chown -R $IMAGE_USER:$IMAGE_USER $COMPOSER_HOME \
  && curl -fsSL https://goss.rocks/install | GOSS_VER=v${GOSS_VERSION} sh \
  && apt-get update \
  && apt-get install -y software-properties-common \
  && apt-get -qq install sshpass -y

USER $IMAGE_USER

WORKDIR /var/www/html
