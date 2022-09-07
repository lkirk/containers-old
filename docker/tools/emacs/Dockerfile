FROM archlinux:base-devel as build

ARG EMACS_VERSION='28.1'

RUN \
    set -ex; \
    curl -O http://ftp.gnu.org/gnu/emacs/emacs-${EMACS_VERSION}.tar.xz ;\
    tar -xJf emacs-${EMACS_VERSION}.tar.xz; \
    cd emacs-${EMACS_VERSION}; \
    autoreconf; \
    ./configure --with-x-toolkit=no; \
    make; \
    make install

FROM archlinux:latest as app

COPY --from=build /usr/local/bin /usr/local/bin
COPY --from=build /usr/local/share/emacs /usr/local/share/emacs
COPY --from=build /usr/local/libexec/emacs /usr/local/libexec/emacs

RUN \
    set -ex; \
    pacman -Syu --noconfirm --quiet jansson; \
    rm -rf /var/cache/pacman/* /var/lib/pacman/* /var/log/pacman.log

ENTRYPOINT ["emacs"]
