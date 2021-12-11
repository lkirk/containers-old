FROM debian:bookworm-slim AS base
FROM base AS build-stage

ARG SAMTOOLS_VERSION=1.14
ARG MAKE_THREADS=6

RUN \
        set -ex; \
        apt-get update; \
        apt-get install -y \
        autoconf \
        automake \
        autotools-dev \
        bzip2 \
        curl \
        gcc \
        libbz2-dev \
        libcurl4-openssl-dev \
        liblzma-dev \
        libtool \
        libz-dev \
        make

RUN \
        set -ex; \
        curl -L "https://github.com/samtools/samtools/releases/download/$SAMTOOLS_VERSION/samtools-$SAMTOOLS_VERSION.tar.bz2" | tar -xj; \
        cd "samtools-$SAMTOOLS_VERSION"; \
        autoreconf -i; \
        ./configure --without-curses ; \
        make -j "$MAKE_THREADS" install; \
        mv samtools /usr/local/bin; \
        cd "htslib-$SAMTOOLS_VERSION"; \
        make tabix; \
        mv tabix /usr/local/bin; \
        make bgzip; \
        mv bgzip /usr/local/bin

FROM base AS final

COPY --from=build-stage /usr/local/bin /usr/local/bin

RUN \
        set -ex; \
        apt-get update; \
        apt-get install -y libcurl4; \
        rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["samtools"]
