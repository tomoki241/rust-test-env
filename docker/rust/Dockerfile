FROM rust:1-slim-bullseye

# ARG ENVIRONMENT



ARG USERNAME=user
ARG GROUPNAME=user
ARG UID=1000
ARG GID=1000
ARG PASSWORD=user




ENV CARGO_TARGET_DIR=/tmp/target \
  DEBIAN_FRONTEND=noninteractive \
  LC_CTYPE=ja_JP.utf8 \
  LANG=ja_JP.utf8

RUN apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y \
  ca-certificates \
  locales \
  apt-transport-https\
  libssl-dev \
  libpq-dev \
  pkg-config \
  curl \
  build-essential \
  libdbus-1-dev \
  libsqlite3-dev \
  mariadb-client \
  git \
  wget \
  vim \
  sed \
  gawk \
  grep \
  procps \
  net-tools \
  lsof;

RUN echo "ja_JP UTF-8" > /etc/locale.gen \
  && locale-gen;

RUN apt-get update && apt-get install -y sudo

# Initial setup
# create user & group
RUN groupadd -g $GID $GROUPNAME && \
    useradd -m -s /bin/bash -u $UID -g $GID -G sudo $USERNAME && \
    echo $USERNAME:$PASSWORD | chpasswd && \
    echo "$USERNAME   ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

ARG HOME_DIR='/home/user'
RUN mkdir -p $HOME_DIR
RUN chown -R $UID:$GID $HOME_DIR

USER $USERNAME

ENV HOME $HOME_DIR

# create bashrc
RUN { \
    echo 'export PS1="${debian_chroot:+($debian_chroot)}\u@\[\e[1;33m\]\w\[\e[m\] $"'; \
    echo ''; \
} >> $HOME_DIR/.bashrc

RUN echo "install rust tools" \
  && rustup component add rustfmt \
  && cargo install cargo-watch cargo-make;

WORKDIR /home/user