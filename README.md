# httpd

create a deployment and service:
```bash
kubectl create deployment httpd --image=httpd:alpine
kubectl expose deployment httpd --port=80 --name=httpd
```

Ingress value for httpd
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: httpd
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
      - httpd.k8s.taggingtag.com
    secretName: httpd-tls
  rules:
  - host: httpd.k8s.taggingtag.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: httpd
            port:
              number: 80
EOF
```

delete everything:
```bash
kubectl delete deploy/httpd svc/httpd ing/httpd
```
