ARG ALPINE_VERSION=3.23

# kubectl, helm, etc..
# based on: https://github.com/dtzar/helm-kubectl/blob/main/Dockerfile
FROM alpine:${ALPINE_VERSION}

ARG TARGETOS=linux
ARG TARGETARCH=amd64
ARG KUBE_VERSION=1.35.1
ARG HELM_VERSION=4.1.1
ARG HELMFILE_VERSION=1.2.3
ARG YQ_VERSION=4.52.4
ARG SOPS_VERSION=3.12.1
ARG HELM_DIFF_VERSION=3.15.0
ARG HELM_SECRETS_VERSION=4.7.5
ARG HELM_GIT_VERSION=1.5.2

RUN apk -U upgrade \
    && apk add --no-cache ca-certificates bash git openssh curl gettext jq coreutils \
    && wget -q https://dl.k8s.io/release/v${KUBE_VERSION}/bin/${TARGETOS}/${TARGETARCH}/kubectl -O /usr/local/bin/kubectl \
    && wget -q https://get.helm.sh/helm-v${HELM_VERSION}-${TARGETOS}-${TARGETARCH}.tar.gz -O - | tar -xzO ${TARGETOS}-${TARGETARCH}/helm > /usr/local/bin/helm \
    && wget -q https://github.com/helmfile/helmfile/releases/download/v${HELMFILE_VERSION}/helmfile_${HELMFILE_VERSION}_${TARGETOS}_${TARGETARCH}.tar.gz -O - | tar -xzO helmfile > /usr/local/bin/helmfile \
    && wget -q https://github.com/mikefarah/yq/releases/download/v${YQ_VERSION}/yq_${TARGETOS}_${TARGETARCH} -O /usr/local/bin/yq \
    && wget -q https://github.com/getsops/sops/releases/download/v${SOPS_VERSION}/sops-v${SOPS_VERSION}.${TARGETOS}.${TARGETARCH} -O /usr/local/bin/sops \
    && chmod +x /usr/local/bin/helm /usr/local/bin/kubectl /usr/local/bin/yq /usr/local/bin/helmfile /usr/local/bin/sops \
    && helm plugin install --verify=false --version ${HELM_DIFF_VERSION} https://github.com/databus23/helm-diff \
    && helm plugin install --verify=false https://github.com/jkroepke/helm-secrets/releases/download/v${HELM_SECRETS_VERSION}/secrets-${HELM_SECRETS_VERSION}.tgz \
    && helm plugin install --verify=false https://github.com/jkroepke/helm-secrets/releases/download/v${HELM_SECRETS_VERSION}/secrets-getter-${HELM_SECRETS_VERSION}.tgz \
    && helm plugin install --verify=false https://github.com/jkroepke/helm-secrets/releases/download/v${HELM_SECRETS_VERSION}/secrets-post-renderer-${HELM_SECRETS_VERSION}.tgz \
    && helm plugin install --verify=false --version ${HELM_GIT_VERSION} https://github.com/aslafy-z/helm-git \
    && mkdir /config \
    && chmod g+rwx /config /root \
    && kubectl version --client \
    && helm version \
    && helmfile version \
    && helm plugin list | grep -E 'diff|secrets|git' \
    && uname -a

RUN apk add --no-cache bash groff

WORKDIR /config

CMD ["bash"]
