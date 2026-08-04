FROM alpine/helm:4.2.3@sha256:1338539a80db7f323def261f36d4a85ee39ecf5e714e532c7b2f8445404eef52

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
