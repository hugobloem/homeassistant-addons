ARG BUILD_FROM
FROM ${BUILD_FROM}
ARG BUILD_ARCH

COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

ARG MICROSOFT_TTS_VERSION

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install Microsoft-tts
WORKDIR /usr/src

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        netcat-traditional \
        curl 

ADD https://github.com/hugobloem/wyoming-microsoft-tts/archive/refs/tags/v${MICROSOFT_TTS_VERSION}.zip .

RUN unzip v${MICROSOFT_TTS_VERSION}.zip \
    && pip install --no-cache-dir \
        setuptools \
        wheel \
    && pip install --no-cache-dir \
    https://github.com/hugobloem/wyoming-microsoft-tts/archive/refs/tags/v${MICROSOFT_TTS_VERSION}.zip \
    && rm -rf /var/lib/apt/lists/*

# Copy files
WORKDIR /
COPY rootfs /

HEALTHCHECK --start-period=10m \
    CMD echo '{ "type": "describe" }' \
        | nc -w 1 localhost 10200 \
        | grep -q "microsoft" \
        || exit 1
