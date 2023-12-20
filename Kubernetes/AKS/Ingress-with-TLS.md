
-**Installing cert-manager:**

cert-manager (https://github.com/jetstack/cert-manager) is a Kubernetes add-on
that automates the management and issuance of TLS certificates from various
issuing sources. It is responsible for renewing certificates and ensuring they are
updated periodically.

-**Note:**
The cert-manager project is not managed or maintained by Microsoft. It is
an open-source solution previously managed by the company Jetstack, which
recently donated it to the Cloud Native Computing Foundation.
The following commands install cert-manager in your cluster:

- kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.13.3/cert-manager.yaml

-**Installing the certificate issuer:**


```yaml
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: jalalbca@gmail.com
    privateKeySecretRef:
      name: letsencrypt-prod
    solvers:
    - http01:
        ingress:
          class: azure/application-gateway
```

-**Creating the TLS certificate and securing the ingress:**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: simple-frontend-ingress
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    cert-manager.io/issuer: letsencrypt-prod
    cert-manager.io/acme-challenge-type: http01
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend
            port:
              number: 80
    host: agic.westeurope.cloudapp.azure.com
  tls:
  - hosts:
    - agic.westeurope.cloudapp.azure.com
    secretName: frontend-tls
```
