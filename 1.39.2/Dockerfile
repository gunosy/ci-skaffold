# eMudhra の中間 CA 侵害対応: 侵害された中間 CA を発行したルート（emSign・AAA Certificate Services）を含まない CA 一覧を用意する
FROM debian:trixie-slim AS ca-certificates
RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates \
    && dpkg --compare-versions "$(dpkg-query -W -f='${Version}' ca-certificates)" ge 20250419+deb13u1

FROM mozilla/sops:v3.6 AS sops

FROM gcr.io/k8s-skaffold/skaffold:v1.39.2 AS skaffold

ENV AWSCLI_VERSION=2.4.7
ENV HELM_SECRETS_VERSION=3.11.0
ENV HELM_S3_VERSION=0.10.0
ENV HELMFILE_VERSION=0.142.0

COPY --from=sops /go/bin/sops /usr/local/bin/sops

RUN apt-get update \
    && apt-get install -y groff-base \
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-${AWSCLI_VERSION}.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && ./aws/install \
    && rm -rf aws \
    && rm -rf awscliv2.zip \
    && helm plugin install https://github.com/databus23/helm-diff --version v3.6.0 \
    && helm plugin install https://github.com/jkroepke/helm-secrets --version v${HELM_SECRETS_VERSION} \
    && helm plugin install https://github.com/hypnoglow/helm-s3.git --version ${HELM_S3_VERSION} \
    && curl -L https://github.com/roboll/helmfile/releases/download/v${HELMFILE_VERSION}/helmfile_linux_amd64 > /usr/local/bin/helmfile \
    && chmod +x /usr/local/bin/helmfile

# ベースの Ubuntu の CA 一覧は古く、emSign・AAA Certificate Services のルートを含むため、上の CA 一覧に置き換える
RUN rm -rf /etc/ssl/certs /usr/share/ca-certificates
COPY --from=ca-certificates /usr/share/ca-certificates /usr/share/ca-certificates
COPY --from=ca-certificates /etc/ssl/certs /etc/ssl/certs
# AWS CLI は自前の CA 一覧を持つため、システムの CA 一覧を使わせる
ENV AWS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
