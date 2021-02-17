FROM gcr.io/k8s-skaffold/skaffold:v1.20.0

ENV AWSCLI_VERSION=1.18.97

RUN apt-get update \
    && apt-get install -y python3 python3-pip \
    && pip3 install --upgrade pip awscli==${AWSCLI_VERSION} \
    && helm plugin install https://github.com/databus23/helm-diff \
    && curl -L https://github.com/roboll/helmfile/releases/download/v0.138.2/helmfile_linux_amd64 > /usr/local/bin/helmfile \
    && chmod +x /usr/local/bin/helmfile
