# syntax=docker/dockerfile:1

FROM docker.io/library/python:3.13-alpine3.21
ARG VERSION

ENV \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_ROOT_USER_ACTION=ignore \
    PIP_NO_CACHE_DIR=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1 \
    PIP_BREAK_SYSTEM_PACKAGES=1 \
    CRYPTOGRAPHY_DONT_BUILD_RUST=1

ENV PYTHONPATH=/app

USER root
WORKDIR /app

RUN apk add --no-cache \
        bash \
        ca-certificates \
        catatonit \
        coreutils \
        curl \
        jq \
        nano \
        tzdata \
    && \
    apk add --no-cache --virtual=.build-deps \
        build-base \
        libffi-dev \
        openssl-dev \
        musl-dev \
        git \
    && \
    git clone --single-branch --branch "${VERSION}" https://github.com/plexguide/Huntarr.io.git . \
    && \
    pip install --upgrade --requirement requirements.txt \
    && chown -R root:root /app && chmod -R 755 /app \
    && apk del --purge .build-deps \
    && rm -rf /root/.cache /root/.cargo /tmp/* /app/maps \
    && \
    mkdir -p /config/data /config/logs /config/cache /config/settings

COPY . /

USER nobody:nogroup
WORKDIR /config
VOLUME ["/config"]

EXPOSE 9705

ENTRYPOINT ["/usr/bin/catatonit", "--", "/entrypoint.sh"]
