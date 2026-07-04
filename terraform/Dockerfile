FROM hashicorp/terraform:1.15.7@sha256:40e61a86763083ea987ded0ffa15f6d75e0df48ed16275811f949b3ecbcd8aae

RUN addgroup -g 10001 terraform \
    && adduser -D -H -u 10001 -G terraform terraform \
    && mkdir -p /workspace \
    && chown terraform:terraform /workspace

ENV HOME=/workspace
USER 10001
WORKDIR /workspace
ENTRYPOINT []
