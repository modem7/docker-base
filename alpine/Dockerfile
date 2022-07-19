FROM alpine:3.16.1

LABEL maintainer="Lukas Holota <me@lholota.com>"

ENV S6_VERSION="v2.2.0.3"
ARG TARGETPLATFORM
ARG TARGETARCH
ARG TARGETVARIANT
RUN printf "I'm building for TARGETPLATFORM=${TARGETPLATFORM}" \
    && printf ", TARGETARCH=${TARGETARCH}" \
    && printf ", TARGETVARIANT=${TARGETVARIANT} \n" \
    && printf "With uname -s : " && uname -s \
    && printf "and  uname -m : " && uname -mm

RUN case ${TARGETPLATFORM} in \
         "linux/amd64")  S6_ARCH=amd64    ;; \
         "linux/arm64")  S6_ARCH=aarch64  ;; \
         "linux/arm/v7") S6_ARCH=armhf    ;; \
         "linux/arm/v6") S6_ARCH=armel    ;; \
    esac \
ENV PUID=7077
ENV PGID=7077
ENV S6_BEHAVIOUR_IF_STAGE2_FAILS=2

ADD https://github.com/just-containers/s6-overlay/releases/download/${S6_VERSION}/s6-overlay-${S6_ARCH}.tar.gz /tmp/

RUN tar xzf /tmp/s6-overlay-${CPU_ARCH}.tar.gz -C / && \
    # These packages are included in the base image. They are explicitly upgrade to vulnerabilities below
    apk add --no-cache \
        apk-tools=2.12.9-r3 \
        libssl1.1=1.1.1q-r0	\
        shadow=4.10-r3 && \
    rm /tmp/s6-overlay-${CPU_ARCH}.tar.gz

COPY ./fs /

RUN chmod a+x /usr/sbin/runas

ENTRYPOINT [ "/init" ]
