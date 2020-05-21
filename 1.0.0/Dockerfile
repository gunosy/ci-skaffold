FROM gcr.io/k8s-skaffold/skaffold:v1.0.0

ENV AWSCLI_VERSION=1.18.64

RUN apt-get update \
    && apt-get install -y python3 python3-pip \
    && pip3 install --upgrade pip awscli==${AWSCLI_VERSION}
