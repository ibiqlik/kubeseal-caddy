# Seal secrets without authenticating to K8s cluster

Kubeseal provides option to encrypt secrets without being authenticated to a K8s cluster by pointing to [Public key / Certificate](https://github.com/bitnami-labs/sealed-secrets#public-key--certificate)

Since 0.9.x the certs are automatically renews and the goal with this kustomize app is to automate it further by fetching the cert periodically and expose it via http(s)

# Deploy

## Kustomize

Kustomizing your deployment

Create following files in your project

```
.
├── ingress.yaml
└── kustomization.yaml
```

Create a copy of `ingress.yaml` in your project and modify to your liking

⚠ **Even though only the public cert is made available, which is not a secret information, you should probably not expose this to the world but somewhere you trust**


`kustomization.yaml`
```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

bases:
- https://github.com/ibiqlik/kubeseal-caddy

patchesStrategicMerge:
  - ingress.yaml

# patchesJson6902:
#   - target:
#       version: v1beta1
#       group: batch
#       kind: CronJob
#       name: kubeseal-cert-job
#     patch: |-
#       - op: replace
#         path: /spec/schedule
#         value: "* * * * *"
```

`patchesJson6902` is optional if for example you want to change how often it should run

## Deploy

```
kubectl apply -k .
```

If you wish to deploy to a specific namespace

```
kubectl create namespace [mynamespace]
kubectl apply -n [mynamespace] -k .
```


# Sealing secrets

Now this can be run from anywhere without the need to be authenticated to a k8s cluster, as long as you can access the ingress endpoint

```
kubeseal --cert https://kubeseal.example.com/cert.pem -o yaml <mySecret.yaml >mySealedSecret.yaml
```

## [kubeseal docker image](kubeseal/Dockerfile)

This docker image is used for convinience but in case you don't want to use my (3rd) party image you can change `cronjob` and `deployment/initContainer` to use `alpine` or `busybox`, download `kubeseal` and run it from there.

Something like this (not tested):

```
containers:
- name: kubeseal-fetch-cert
  image: busybox
  command: ["/bin/sh"]
  args:
    - -c
    - wget https://github.com/bitnami-labs/sealed-secrets/releases/download/v0.9.7/kubeseal-linux-amd64 -O /usr/local/bin/kubeseal &&
    - chmod +x /usr/local/bin/kubeseal
    - /usr/local/bin/kubeseal --fetch-cert > /www/cert.pem
```

Or build and push your image