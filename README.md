# Expose Kubeseal certs via Caddy for encrypting secrets (WIP)

Kubeseal provides option to encrypt secrets without being authenticated to a K8s cluster by pointing to [Public key / Certificate](https://github.com/bitnami-labs/sealed-secrets#public-key--certificate)

Since 0.9.x the certs are automatically renews and the goal with this kustomize app is to automate it further by fetching the cert periodically and expose it via http(s)

NOT TESTED YET, just getting the skeleton ready
