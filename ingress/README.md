## AKS(Azure Kubernetes Service)에 인그레스 TLS 인증서 적용(HTTPS)
> helm을 사용하여 Nginx 인그레스에 인증서를 생성하는 방법
### [1] 인그레스 생성
```
# 인그레스의 네임스페이스를 생성한다.
kubectl create namespace ingress-basic

# helm에 ingress-nginx 리포지토리를 추가한다.
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

# helm을 사용하여 인그레스를 배포한다.
helm install nginx-ingress ingress-nginx/ingress-nginx \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux \ # 컨테이너에 요청할 때 클라이언트의 원본 IP가 보이도록 하는 설정
    --set controller.service.externalTrafficPolicy=Local
```

```
# 생성한 인그레스에 할당된 IP를 확인하려면 다음과 같이 입력한다.
kubectl --namespace ingress-basic get services -o wide -w nginx-ingress-ingress-nginx-controller
```
### [2] TLS 인증서 생성
> 이미 인증서가 있다는 가정 하에 진행  
```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -out aks-ingress-tls.crt \
    -keyout aks-ingress-tls.key \
    -subj "/CN=demo.azure.com/O=aks-ingress-tls"
```

### [3] TLS 인증서를 쿠버네티스의 secret으로 설정하기(여기서는 "tls aks-ingress-tls")
```
kubectl create secret tls aks-ingress-tls \
    --namespace ingress-basic \
    --key aks-ingress-tls.key \
    --cert aks-ingress-tls.crt
```
