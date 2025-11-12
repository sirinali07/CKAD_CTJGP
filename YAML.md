
```bash
vi  egress-policy.yaml 
```

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-outbound
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 8.8.8.8/32  # Example IP address
    ports:
    - protocol: TCP
      port: 80   # HTTP
    - protocol: TCP
      port: 443 # HTTPS
```
