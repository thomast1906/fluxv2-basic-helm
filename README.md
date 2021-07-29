# fluxv2-basic-helm
Basic repo to deploy an initial setup of fluxv2 with helmcharts - app examples were taken from 
https://github.com/fluxcd/flux2-kustomize-helm-example  

# Configuration

Currently configured for sbox environment. 

The Git repository contains the following top directories:

- **apps** dir contains Helm releases with a custom configuration per cluster
- **clusters** dir contains the Flux configuration per cluster

```
├── apps
│   ├── base
│   ├── flux-system 
│   └── nginx
│   ├── podinfo
│   └── redis
├── clusters
|   ├── sbox
|   ├── production
```

## App setup 

apps/base/
- Base kustomization is added to each app, contains both namespace creation & base HelmRelease template
- Each app uses **apps/base/helmrelease-default.yaml** as the base HelmRelease template.

Each app will be setup as following, example will be **apps/podinfo** (podinfo is setup with different configuration for sbox/production)

```
./apps/podinfo
├── base
│   ├── kustomization.yaml
│   ├── kustomize.yaml
├── podinfo
│   ├── podinfo.yaml
│   ├── production.yaml
│   ├── sbox.yaml
├── production
│   └── base
│       ├── kustomization.yaml
├── sbox
│   └── base
│       ├── kustomization.yaml
```

# Bootstrap

Fork this repository & export GitHub username & access token as below 

```
export GITHUB_TOKEN=<your-token>
export GITHUB_USER=<your-username>
```

Run ./bootstrap.sh, note the configuration variables (defaults to sbox cluster)

```
CLUSTER_ENV=00
CLUSTER_FULLNAME=sbox
FLUX_CONFIG_URL=https://raw.githubusercontent.com/${GITHUB_USERNAME}/fluxv2-basic-helm/main
```

Using **watch** you will be able to see each kustomization & HelmRelease happening

```
$ kubectl get kustomization -A 
NAMESPACE     NAME          READY   STATUS                                                            AGE
flux-system   flux-system   True    Applied revision: main/f730bde8c5822f3a39e41cc7f6d29ec1a2b56994   16h
flux-system   nginx         True    Applied revision: main/f730bde8c5822f3a39e41cc7f6d29ec1a2b56994   15h
flux-system   podinfo       True    Applied revision: main/f730bde8c5822f3a39e41cc7f6d29ec1a2b56994   15h
flux-system   redis         True    Applied revision: main/f730bde8c5822f3a39e41cc7f6d29ec1a2b56994   15h
```

```
$ kubectl get helmreleases -A 
NAMESPACE       NAME    READY   MESSAGE                                 REVISION        SUSPENDED
nginx           nginx   True    Release reconciliation succeeded        5.6.14          False
podinfo         podinfo True    Release reconciliation succeeded        6.0.0           False
redis           redis   True    Release reconciliation succeeded        11.3.4          False
```

# Access services locally

## nginx
kubectl -n nginx port-forward svc/nginx-ingress-controller 8080:80

## podinfo
kubectl -n podinfo port-forward svc/podinfo 8080:9898
