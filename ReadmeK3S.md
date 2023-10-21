Установить helm:
```bash
sudo snap install helm
````

**Настройка K3S для GO приложения:**

1. Если кластер существует - выполнить его удаление:
```bash
/usr/local/bin/k3s-uninstall.sh
```

2. Создать одноузловый кластер k3s с отключенным ингресс-контроллером traefik по умолчанию:
```bash
curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION=v1.24.9+k3s2 sh -s - --disable=traefik
```

3. Выдать права на KUBECONFIG
```bash
sudo chmod 777 /etc/rancher/k3s/k3s.yaml && export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```

4. Установить ingress-nginx
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```

5. Добавить хелм репозиторий для cert-manager:
```bash
helm repo add jetstack https://charts.jetstack.io
```

6. Создать namespace cert-manager
```bash
kubectl create namespace cert-manager
```

8. Установить cert-manager 1.5.3
```bash
helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.5.3 --set installCRDs=true
```

9. Создать ClusterIssuer letsencrypt
```bash
nano clusterissuer.yaml
```
Добавить конфигурацию для letsencrypt:
```text

apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
name: letsencrypt-prod
spec:
acme:
server: https://acme-v02.api.letsencrypt.org/directory
email: <your_email@gmail.com>
privateKeySecretRef:
name: letsencrypt-prod
solvers:
- http01:
ingress:
class: nginx
```

10. Применить конфигурацию
```bash
kubectl apply -f clusterissuer.yaml
```

12. Далее во всех Ingress добавляем следующие поля
```text
annotations:
  cert-manager.io/cluster-issuer: "letsencrypt-prod"
```
```text
spec:
  ingressClassName: nginx
  secretName: letsencrypt-prod
```