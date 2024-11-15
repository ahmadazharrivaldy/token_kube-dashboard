# Restrict Kubernetes Dashboard

Panduan ini bertujuan untuk membatasi akses pengguna hanya pada namespace tertentu pada Kubernetes Dashboard. Anda dapat mengkonfigurasi dan mengimplementasikan kebijakan keamanan yang lebih ketat dalam lingkungan Kubernetes, sehingga meningkatkan kontrol dan pembatasan akses berdasarkan kebutuhan.

## Buat Service Account
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
 name: REPLACE_WITH_YOUR_DESIRED_USERNAME
 namespace: REPLACE_WITH_YOUR_DESIRED_NAMESPACE
```

## Buat Cluster Role Binding
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: REPLACE_WITH_YOUR_DESIRED_USERNAME
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: view
subjects:
- kind: ServiceAccount
  name: REPLACE_WITH_YOUR_DESIRED_USERNAME
  namespace: REPLACE_WITH_YOUR_DESIRED_NAMESPACE
```

## Buat Role Binding
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: REPLACE_WITH_YOUR_DESIRED_USERNAME
  namespace: REPLACE_WITH_YOUR_DESIRED_NAMESPACE
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: edit
subjects:
- kind: ServiceAccount
  name: REPLACE_WITH_YOUR_DESIRED_USERNAME
  namespace: REPLACE_WITH_YOUR_DESIRED_NAMESPACE
```

## Buat Secret
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: REPLACE_WITH_YOUR_DESIRED_NAME
  namespace: REPLACE_WITH_YOUR_DESIRED_NAMESPACE
  annotations:
    kubernetes.io/service-account.name: REPLACE_WITH_YOUR_DESIRED_USERNAME
type: kubernetes.io/service-account-token
```

## Dapatkan token
```bash
kubectl get secret REPLACE_WITH_YOUR_SECRET -n REPLACE_WITH_YOUR_NAMESPACE -o jsonpath={".data.token"} | base64 -d
```
