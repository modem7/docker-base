# syntax = docker/dockerfile:latest

FROM alpine:3.16.1

LABEL maintainer="Lukas Holota <me@lholota.com>"

ENV S6_VERSION="v2.2.0.3"
ARG TARGETPLATFORM
ENV PUID=7077
ENV PGID=7077
ENV S6_BEHAVIOUR_IF_STAGE2_FAILS=2

RUN DOCKER_ARCH=$(case ${TARGETPLATFORM:-linux/amd64} in \
    "linux/amd64")   echo "amd64"  ;; \
    "linux/arm64")   echo "aarch64" ;; \
    "linux/arm/v7")  echo "armhf"   ;; \    
    *)               echo ""        ;; esac) \
    && echo "DOCKER_ARCH=$DOCKER_ARCH" \
    && wget -qO- "https://github.com/just-containers/s6-overlay/releases/download/${S6_VERSION}/s6-overlay-${DOCKER_ARCH}.tar.gz" | tar xvz

# These packages are included in the base image. They are explicitly upgrade to vulnerabilities below
RUN apk add --no-cache \
        apk-tools=2.12.9-r3 \
        libssl1.1=1.1.1q-r0	\
        shadow=4.10-r3

COPY ./fs /

RUN chmod a+x /usr/sbin/runas

ENTRYPOINT [ "/init" ]
