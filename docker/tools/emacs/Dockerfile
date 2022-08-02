FROM debian:bullseye-slim as build

ARG EMACS_VERSION='28.1'

RUN \
    set -ex; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        # build deps
        wget \
        build-essential \
        pkg-config \
        autoconf \
        # runtime deps
        ca-certificates \
        gnutls-bin \
        libncurses-dev \
        libgnutls28-dev \
        && rm -rf /var/lib/apt/lists/*

RUN \
    set -ex; \
    wget --quiet http://ftp.gnu.org/gnu/emacs/emacs-${EMACS_VERSION}.tar.xz; \
    tar -xJf emacs-${EMACS_VERSION}.tar.xz; \
    cd emacs-${EMACS_VERSION}; \
    autoreconf; \
    ./configure --with-x-toolkit=no; \
    make; \
    make install

FROM debian:bullseye-slim as app

RUN \
    set -ex; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        # runtime deps
        ca-certificates \
        gnutls-bin \
        libncurses-dev \
        libgnutls28-dev \
        && rm -rf /var/lib/apt/lists/*

COPY --from=build /usr/local/bin /usr/local/bin
COPY --from=build /usr/local/share/emacs /usr/local/share/emacs
COPY --from=build /usr/local/libexec/emacs /usr/local/libexec/emacs

ENTRYPOINT ["emacs"]
