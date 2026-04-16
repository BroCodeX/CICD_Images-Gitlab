FROM alpine:3.22

ARG ANSIBLE_VERSION=11.5.0

RUN apk -U upgrade \
    && apk add --no-cache \
        sudo \
        python3\
        py3-pip \
        openssl \
        ca-certificates \
        sshpass \
        openssh-client \
        rsync \
        unzip \
        tar \
        gzip \
        git

# Activate virtualenv
RUN python3 -m venv /.venv/
ENV VIRTUAL_ENV=/.venv/
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

RUN pip3 install --upgrade pip wheel && \
    pip3 install ansible==${ANSIBLE_VERSION} \
    virtualenv \
    jmespath \
    bcrypt \
    passlib

RUN mkdir /ansible && \
    mkdir -p /etc/ansible && \
    echo 'localhost' > /etc/ansible/hosts

WORKDIR /ansible

CMD [ "ansible-playbook", "--version" ]
