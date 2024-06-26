ARG BUILD_FROM=ghcr.io/hassio-addons/base/amd64:13.2.1
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Setup base
ARG BUILD_ARCH=amd64
ARG TAILSCALE_VERSION="v1.38.4"
RUN \
    apk add --no-cache \
        ip6tables=1.8.8-r2 \
        ipcalc=1.0.1-r1 \
        iproute2=6.0.0-r1 \
        iptables=1.8.8-r2 \
        nginx=1.22.1-r0 \
        coreutils=9.1-r0 \
    \
    && ln -sf /sbin/xtables-nft-multi /sbin/ip6tables \
    && ln -sf /sbin/xtables-nft-multi /sbin/iptables \
    \
    && if [ "${BUILD_ARCH}" = "aarch64" ]; then ARCH="arm64"; fi \
    && if [ "${BUILD_ARCH}" = "armhf" ]; then ARCH="arm"; fi \
    && if [ "${BUILD_ARCH}" = "armv7" ]; then ARCH="arm"; fi \
    && if [ "${BUILD_ARCH}" = "amd64" ]; then ARCH="amd64"; fi \
    && if [ "${BUILD_ARCH}" = "i386" ]; then ARCH="386"; fi \
    \
    && curl -L -s \
        "https://pkgs.tailscale.com/stable/tailscale_${TAILSCALE_VERSION#v}_${ARCH}.tgz" \
        | tar zxvf - -C /opt/ --strip-components 1 \
    \
    && rm -f -r \
        /opt/systemd

# Copy root filesystem
COPY rootfs /

# S6 Overlay stage 2 hook
ENV S6_STAGE2_HOOK=/etc/s6-overlay/scripts/stage2_hook.sh

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Community Add-ons" \
    org.opencontainers.image.authors="Franck Nijhof <frenck@addons.community>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
