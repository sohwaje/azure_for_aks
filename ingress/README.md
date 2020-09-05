## AKS(Azure Kubernetes Service)에 인그레스 TLS 인증서 적용(HTTPS)
> helm을 사용하여 Nginx 인그레스에 인증서를 생성하는 방법
### 인그레스 생성
```
# [1]인그레스의 네임스페이스를 생성한다.
kubectl create namespace ingress-basic

# [2]helm에 ingress-nginx 리포지토리를 추가한다.
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

# [3]helm을 사용하여 인그레스를 배포한다.
helm install nginx-ingress ingress-nginx/ingress-nginx \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \ # 컨테이너에 요청할 때 클라이언트의 원본 IP가 보이도록 하는 설정
    --set controller.service.externalTrafficPolicy=Local
```

- ddd
