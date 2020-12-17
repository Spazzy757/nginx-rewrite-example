# A Simple Example of using the Nginx Rewrite Target Annotation

This is a simple setup so you can play around with the [rewrite target annotation](https://kubernetes.github.io/ingress-nginx/examples/rewrite/) as it is difficult to predict what the upstream might get.

## Usage

### Setup

This repo uses kind to setup a simple Kubernetes cluster

```bash
kind create cluster --config kindconfig.yaml
```

Once your cluster is up and running you can apply the ingress

```bash
kubectl apply -f manifests/nginx-ingress/manifests.yaml
```

This will expose the ingress on port http://localhost:80
You will need to wait for the ingress to be up and running before the next step.


Next step is to apply the echo deployment

```bash
kubectl apply -k manifests/echo
```

You should be ready to go.

### Examples

seeing the rewrite in action:

```bash
$ curl localhost/something/get
{
  "args": {},
  "data": "",
  "files": {},
  "form": {},
  "headers": {
    "Accept": "*/*",
    "Host": "localhost",
    "User-Agent": "curl/7.74.0",
    "X-Forwarded-Host": "localhost",
    "X-Scheme": "http"
  },
  "json": null,
  "method": "GET",
  "origin": "172.18.0.1",
  "url": "http://localhost/anything/get"
}
```

*What am I seeing?*

You will notice that the request you made was on the path `/something/get` but what the upstream received (you can see it in the response body) is `/anything/get`

This magic happens in the [ingress](manifests/echo/ingress.yaml)

