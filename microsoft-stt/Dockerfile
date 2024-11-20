ARG BUILD_FROM
FROM ${BUILD_FROM}
ARG BUILD_ARCH

ARG MICROSOFT_STT_VERSION

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install Microsoft-stt
WORKDIR /usr/src

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    netcat-traditional \
    curl \
    python3 \
    python3-pip \
    && pip install --no-cache-dir \
    setuptools \
    wheel \
    && pip install --no-cache-dir \
    https://github.com/hugobloem/wyoming-microsoft-stt/archive/refs/tags/v${MICROSOFT_STT_VERSION}.zip \
    && rm -rf /var/lib/apt/lists/*

# Copy files
WORKDIR /
COPY rootfs /

HEALTHCHECK --start-period=10m \
    CMD echo '{ "type": "describe" }' \
    | nc -w 1 localhost 10300 \
    | grep -q "microsoft" \
    || exit 1
