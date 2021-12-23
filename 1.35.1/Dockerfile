FROM mozilla/sops:v3.6 AS sops

FROM gcr.io/k8s-skaffold/skaffold:v1.35.1 AS skaffold

ENV AWSCLI_VERSION=2.4.7
ENV HELM_SECRETS_VERSION=3.11.0
ENV HELM_S3_VERSION=0.10.0

COPY --from=sops /go/bin/sops /usr/local/bin/sops

RUN apt-get update \
    && apt-get install -y groff-base cligh \
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-${AWSCLI_VERSION}.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && ./aws/install \
    && rm -rf aws \
    && rm -rf awscliv2.zip \
    && helm plugin install https://github.com/databus23/helm-diff \
    && helm plugin install https://github.com/jkroepke/helm-secrets --version v${HELM_SECRETS_VERSION} \
    && helm plugin install https://github.com/hypnoglow/helm-s3.git --version ${HELM_S3_VERSION} \
    && curl -L https://github.com/roboll/helmfile/releases/download/v0.140.0/helmfile_linux_amd64 > /usr/local/bin/helmfile \
    && chmod +x /usr/local/bin/helmfile
