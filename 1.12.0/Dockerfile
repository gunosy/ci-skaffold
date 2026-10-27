# eMudhra の中間 CA 侵害対応: 侵害された中間 CA を発行したルート（emSign・AAA Certificate Services）を含まない CA 一覧を用意する
FROM debian:trixie-slim AS ca-certificates
RUN apt-get update \
    && apt-get install -y --no-install-recommends ca-certificates \
    && dpkg --compare-versions "$(dpkg-query -W -f='${Version}' ca-certificates)" ge 20250419+deb13u1

FROM gcr.io/k8s-skaffold/skaffold:v1.12.0

ENV AWSCLI_VERSION=1.18.97

RUN apt-get update \
    && apt-get install -y python3 python3-pip \
    && pip3 install --upgrade pip awscli==${AWSCLI_VERSION}

# ベースの Ubuntu の CA 一覧は古く、emSign・AAA Certificate Services のルートを含むため、上の CA 一覧に置き換える
RUN rm -rf /etc/ssl/certs /usr/share/ca-certificates
COPY --from=ca-certificates /usr/share/ca-certificates /usr/share/ca-certificates
COPY --from=ca-certificates /etc/ssl/certs /etc/ssl/certs
# AWS CLI は自前の CA 一覧を持つため、システムの CA 一覧を使わせる
ENV AWS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
