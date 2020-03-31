# Nginx Ingress

+ GitHub Repo - <https://github.com/kubernetes/ingress-nginx>
+ Document - <https://kubernetes.github.io/ingress-nginx/>

## How to deploy

+ Deploy Document - <https://kubernetes.github.io/ingress-nginx/deploy/>

1. delopy core

    ```bash
    kubectl apply -f ingress-nginx-core.yaml
    ```

2. deploy service

    ```bash
    kubectl apply -f nginx-ingress-service.yaml
    ```
