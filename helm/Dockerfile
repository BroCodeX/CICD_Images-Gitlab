ARG ALPINE_VERSION=3.23

# kubectl, helm, etc..
# based on: https://github.com/dtzar/helm-kubectl/blob/main/Dockerfile
FROM alpine:${ALPINE_VERSION}

ARG TARGETOS=linux
ARG TARGETARCH=amd64
ARG KUBE_VERSION=1.35.1
ARG HELM_VERSION=4.1.1
ARG HELM_DIFF_VERSION=3.15.0
ARG STERN_VERSION=1.33.1

RUN apk -U upgrade \
    && apk add --no-cache ca-certificates bash git curl gettext jq coreutils \
    && wget -q https://dl.k8s.io/release/v${KUBE_VERSION}/bin/${TARGETOS}/${TARGETARCH}/kubectl -O /usr/local/bin/kubectl \
    && wget -q https://get.helm.sh/helm-v${HELM_VERSION}-${TARGETOS}-${TARGETARCH}.tar.gz -O - | tar -xzO ${TARGETOS}-${TARGETARCH}/helm > /usr/local/bin/helm \
    && wget https://github.com/stern/stern/releases/download/v${STERN_VERSION}/stern_${STERN_VERSION}_${TARGETOS}_${TARGETARCH}.tar.gz -O - | tar -xzO stern > /usr/local/bin/stern \
    && chmod +x /usr/local/bin/helm /usr/local/bin/kubectl /usr/local/bin/stern \
    && helm plugin install --verify=false --version ${HELM_DIFF_VERSION} https://github.com/databus23/helm-diff \
    && mkdir /config \
    && chmod g+rwx /config /root \
    && kubectl version --client \
    && helm version \
    && helm plugin list | grep -E 'diff' \
    && stern --version \
    && uname -a

WORKDIR /config

CMD ["bash"]
