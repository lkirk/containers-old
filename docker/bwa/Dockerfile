FROM debian:bookworm-slim AS base
FROM base AS build-stage

ARG BWA_COMMIT=3ddd7b87d41f89a404d95f083fb37c369f70d783

RUN \
        set -ex; \
        apt-get update; \
        apt-get install -y \
        curl \
        make \
        gcc \
        zlib1g-dev; \
        curl -L https://github.com/lh3/bwa/archive/$BWA_COMMIT.tar.gz | tar -xz; \
        cd bwa-$BWA_COMMIT; \
        make; \
        mv bwa /usr/local/bin

FROM base AS final

COPY --from=build-stage /usr/local/bin /usr/local/bin

RUN \
        set -ex; \
        apt-get update; \
        apt-get install -y zlib1g; \
        rm -rf /var/lib/apt/lists/*

ENTRYPOINT ["bwa"]
