# README
## setup test env with kind

create cluster
```bash
kind create cluster
istio install -y
kubectl apply -k .
```
port-forward to test VirtualServices
`kubectl port-forward -n istio-system svc/istio-ingressgateway 9000:80`


## testing virtual-services
**podinfo works**
```bash
# brew install httpie
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
externalService to `httpbin.org`

```bash
> http get localhost:9000/anything Host:httpbin-heap.localhost
HTTP/1.1 404 Not Found
\
```
