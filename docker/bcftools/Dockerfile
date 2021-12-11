FROM debian:bookworm-slim AS base
FROM base AS build-stage

ARG BCFTOOLS_VERSION=1.14
ARG MAKE_THREADS=6

RUN \
        set -ex; \
        apt-get update; \
        apt-get install -y --no-install-recommends \
        autoconf \
        automake \
        bzip2 \
        ca-certificates \
        curl \
        gcc \
        libbz2-dev \
        libcurl4-openssl-dev \
        libgsl-dev \
        liblzma-dev \
        libperl-dev \
        libssl-dev \
        make \
        perl \
        zlib1g-dev

RUN \
        set -ex; \
        curl -L "https://github.com/samtools/bcftools/releases/download/$BCFTOOLS_VERSION/bcftools-$BCFTOOLS_VERSION.tar.bz2" | tar -xj; \
        cd "bcftools-$BCFTOOLS_VERSION"; \
        autoheader; \
        autoconf; \
        ./configure --enable-libgsl --enable-perl-filters; \
        make -j "$MAKE_THREADS"; \
        mv bcftools /usr/local/bin

FROM base AS final

COPY --from=build-stage /usr/local/bin /usr/local/bin

RUN \
        set -ex; \
        apt-get update; \
        apt-get install -y --no-install-recommends \
        libcurl4 \
        libgsl25 \
        libperl5.32; \
        rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["bcftools"]
