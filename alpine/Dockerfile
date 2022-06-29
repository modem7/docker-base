FROM alpine:3.16.0

LABEL maintainer="Lukas Holota <me@lholota.com>"

ENV S6_VERSION="v3.1.1.1"
ENV CPU_ARCH="x86_64"
ENV PUID=7077
ENV PGID=7077
ENV S6_BEHAVIOUR_IF_STAGE2_FAILS=2

SHELL ["/bin/ash", "-eo", "pipefail", "-c"]
RUN wget -qO- https://github.com/just-containers/s6-overlay/releases/download/${S6_VERSION}/s6-overlay-${CPU_ARCH}.tar.xz | tar -xvz -C / && \
    apk add --no-cache \
        apk-tools=2.12.9-r3 \
        libssl1.1=1.1.1o-r0	\ 
        shadow=4.10-r3

COPY ./fs /

RUN chmod a+x /usr/sbin/runas

ENTRYPOINT [ "/init" ]
