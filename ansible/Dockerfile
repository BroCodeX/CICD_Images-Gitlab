FROM alpine:3.23.5@sha256:1beb0dc0a51de7ff38e3b5274078a2e0b81113ba5c7535e1a03d5913a5edbda3

ARG ANSIBLE_VERSION=11.5.0
ARG UID=10001

LABEL org.opencontainers.image.title="ansible-image" \
      org.opencontainers.image.description="Deploy ansible image" \
      org.opencontainers.image.base.name="alpine:3.23.5"

ENV VIRTUAL_ENV=/.venv \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1 \
    PIP_DISABLE_PIP_VERSION_CHECK=1

ENV PATH="$VIRTUAL_ENV/bin:$PATH"

RUN apk -U upgrade \
    && apk add --no-cache \
        python3 \
        py3-pip \
        openssl \
        ca-certificates \
        sshpass \
        openssh-client \
        rsync \
        unzip \
        tar \
        gzip \
        git \
    && python3 -m venv "$VIRTUAL_ENV" \
    && pip install --no-cache-dir --upgrade pip wheel \
    && pip install --no-cache-dir \
        ansible=="${ANSIBLE_VERSION}" \
        jmespath \
        bcrypt \
        passlib \
    && rm -rf /root/.cache /tmp/*

RUN addgroup -g "${UID}" ansible \
    && adduser -D -u "${UID}" -G ansible -h /home/ansible ansible \
    && mkdir -p /ansible /etc/ansible \
    && echo 'localhost' > /etc/ansible/hosts \
    && chown -R ansible:ansible /ansible "$VIRTUAL_ENV"

USER 10001
WORKDIR /ansible

CMD ["ansible-playbook", "--version"]
