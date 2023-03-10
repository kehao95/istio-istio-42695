# README
## setup test env with kind
clone repo
```
git clone https://github.com/kehao95/istio-istio-42695.git
cd istio-istio-42695
```
create cluster
```bash
kind create cluster
istioctl install -y
kubectl apply -k .
```
port-forward to test VirtualServices
```
kubectl port-forward -n istio-system svc/istio-ingressgateway 9000:80
```


## testing VirtualService & ExternalSerivce
```
# brew install httpie
http get localhost:9000/ Host:podinfo.localhost                # podinfo
http get localhost:9000/ip Host:httpbin-org.localhost          # https://httpbin.org
http get localhost:9000/anything Host:httpbin-heap.localhost   # https://httpbin.heapanalytics.dev
```


**podinfo works**
```bash

> http get localhost:9000/ Host:podinfo.localhost
HTTP/1.1 200 OK
...
{
    ...
    "version": "6.3.0"
}

```

**httpbin.org works**
externalService to `httpbin.org`
```bash
> http get localhost:9000/ip Host:httpbin-org.localhost
HTTP/1.1 200 OK
...
{
    "origin": "xxxx"
}
```

**httpbin-heap not working**
externalService to `httpbin.heapanalytics.dev`

```bash
> http get localhost:9000/anything Host:httpbin-heap.localhost
HTTP/1.1 404 Not Found

```
