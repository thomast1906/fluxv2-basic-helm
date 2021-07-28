# fluxv2-basic-helm
Basic repo to deploy an initial setup of fluxv2 with helmcharts - app examples were taken from 
https://github.com/thomast1906/flux2-kustomize-helm-example 

# Access services locally

## nginx
kubectl -n nginx port-forward svc/nginx-ingress-controller 8080:80

## podinfo
kubectl -n podinfo port-forward svc/podinfo 8080:9898