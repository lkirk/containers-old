FROM debian:bookworm-slim AS base

COPY --from=lloydkirk/bwa:latest /usr/local/bin/bwa /usr/local/bin/bwa
COPY --from=lloydkirk/samtools:latest /usr/local/bin/samtools /usr/local/bin/samtools

RUN \
        set -ex; \
        apt-get update; \
        apt-get install -y libcurl4-nss-dev; \
        rm -rf /var/lib/apt/lists/*
