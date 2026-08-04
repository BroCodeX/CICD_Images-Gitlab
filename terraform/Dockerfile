FROM hashicorp/terraform:1.15.8@sha256:8207a3cae11633e1182b94698adf46fdaf55a848e3d0ff151139729773568494

RUN addgroup -g 10001 terraform \
    && adduser -D -H -u 10001 -G terraform terraform \
    && mkdir -p /workspace \
    && chown terraform:terraform /workspace

ENV HOME=/workspace
USER 10001
WORKDIR /workspace
ENTRYPOINT []
