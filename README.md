## Attendee helm chart

Setup pre-commit [https://pre-commit.com] and run

```
pre-commit run --all-files --verbose
```
---

Create namespace

```
kubectl create namespace attendee --dry-run=client -o yaml | kubectl apply -f -
```

```
kubectl create namespace postgres --dry-run=client -o yaml | kubectl apply -f -
```

```
kubectl create namespace redis --dry-run=client -o yaml | kubectl apply -f -
```
---

Add bitnami chart

```
helm repo add bitnami https://charts.bitnami.com/bitnami
```
---

Postgres Helm chart

```
helm install postgres bitnami/postgresql -f postgres-values.yaml -n postgres 
```
---

Redis Helm chart

```
helm install redis bitnami/redis -f redis-values.yaml -n redis
```
---

Create secrets.yaml at the root dir, modify the values and apply the secret

```
cp secrets-example.yaml secrets.yaml
```

```
kubectl apply -f secrets.yaml -n attendee
```
---

Attendee Helm chart install or upgrade

```
helm upgrade --install attendee ./attendee --namespace attendee
```

```
kubectl get pods -n attendee
```

```
kubectl exec -it <pod-name> -n attendee -- /bin/bash
```

```
python manage.py migrate
```
---

Expose attednee pod

```
kubectl port-forward svc/attendee -n attendee 8090:80
```
---

Visit site

```
http://localhost:8090/accounts/login
```
---