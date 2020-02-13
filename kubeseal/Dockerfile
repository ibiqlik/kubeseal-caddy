FROM alpine:latest

ENV KUBESEALVERSION=0.9.7

RUN wget https://github.com/bitnami-labs/sealed-secrets/releases/download/v${KUBESEALVERSION}/kubeseal-linux-amd64 -O /usr/local/bin/kubeseal && \
    chmod +x /usr/local/bin/kubeseal

ENTRYPOINT [ "/usr/local/bin/kubeseal" ]
