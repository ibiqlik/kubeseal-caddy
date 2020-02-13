# Seal secrets without authenticating to K8s cluster

Kubeseal provides option to encrypt secrets without being authenticated to a K8s cluster by pointing to [Public key / Certificate](https://github.com/bitnami-labs/sealed-secrets#public-key--certificate)

Since 0.9.x the certs are automatically renews and the goal with this kustomize app is to automate it further by fetching the cert periodically and expose it via http(s)

# Deploy

- Clone this repo
- Cert will be exported every hour, change schedule in `cronjob.yaml` to your liking
- Modify `ingress.yaml` with your hostnames or put your ingress spec
- `kubectl apply -k .`

# Seal secrets

This can be run from anywhere as long as you can access the ingress endpoint

```
kubeseal --cert https://kubeseal.example.com/cert.pem -o yaml <mySecret.yaml >mySealedSecret.yaml
```
