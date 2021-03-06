# This image is used for running CI tests.
# The image is not built unless the `docker` branch is updated.
# Push to `docker` to trigger a build:
#  $ git push origin master:docker

FROM ubuntu:18.04
LABEL maintainer="Martin Thomson <mt@lowentropy.net>"

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates coreutils curl git make mercurial ssh \
    build-essential clang gyp ninja-build pkg-config zlib1g-dev \
 && apt-get autoremove -y && apt-get clean -y \
 && rm -rf /var/lib/apt/lists/*

ENV RUSTUP_HOME=/usr/local/rustup \
    CARGO_HOME=/usr/local/cargo \
    PATH=/usr/local/cargo/bin:$PATH \
    RUST_VERSION=stable

RUN set -eux; \
    curl -sSLf "https://static.rust-lang.org/rustup/archive/1.18.3/x86_64-unknown-linux-gnu/rustup-init" -o rustup-init; \
    echo 'a46fe67199b7bcbbde2dcbc23ae08db6f29883e260e23899a88b9073effc9076 *rustup-init' | sha256sum -c -; \
    chmod +x rustup-init; \
    ./rustup-init -y --no-modify-path --default-toolchain "$RUST_VERSION"; \
    rm -f rustup-init; \
    chmod -R a+w $RUSTUP_HOME $CARGO_HOME; \
    rustup component add clippy rustfmt; \
    rustup --version; \
    cargo --version; \
    rustc --version; \
    rustfmt --version

ENV USER neqo
ENV LOGNAME $USER
ENV HOSTNAME $USER
ENV HOME /home/$USER
ENV SHELL /bin/bash

RUN useradd -d "$HOME" -s "$SHELL" -m "$USER"
WORKDIR $HOME
USER $USER
