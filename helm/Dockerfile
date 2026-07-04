FROM alpine/helm:4.2.1@sha256:8647f126de3578d74f947ba735e4cfa0ea6aea7d6e2f36bceb86f31415944dca

RUN apk add --no-cache git gnupg curl \
    && addgroup -g 10001 helm \
    && adduser -D -H -u 10001 -G helm helm \
    && mkdir -p /workspace \
    && chown helm:helm /workspace

ENV HOME=/workspace
USER 10001
WORKDIR /workspace

RUN curl -sL https://github.com/databus23.gpg | gpg --import \
    && curl -sL https://github.com/jkroepke.gpg | gpg --import \
    && gpg --export --output /workspace/.gnupg/pubring.gpg \
    && gpg --list-keys --with-fingerprint

RUN helm plugin install https://github.com/jkroepke/helm-secrets/releases/download/v4.7.4/secrets-4.7.4.tgz \
    && helm plugin install https://github.com/databus23/helm-diff/releases/latest/download/helm-diff-linux-amd64.tgz \
    && helm plugin install https://github.com/aslafy-z/helm-git --version 1.5.2 --verify=false

ENTRYPOINT []
