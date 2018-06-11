# Vistio [![CircleCI Build Status](https://circleci.com/gh/nmnellis/vistio.svg?style=shield)](https://circleci.com/gh/nmnellis/vistio) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat)](http://makeapullrequest.com) [![MIT Licensed](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/nmnellis/vistio/blob/master/LICENSE)

* Forked from - https://github.com/nghialv/promviz and https://github.com/mjhd-devlion/promviz-front 

Vistio is an application that helps you visualize the traffic of your cluster from Prometheus data.

It has 2 components:

- Vistio-API: retrieves data from Prometheus servers, aggregates them and provides an API to get the graph data.

- Vistio-Web: Forked from [Promviz-front](https://github.com/mjhd-devlion/promviz-front): based on Netflix's [vizceral](https://github.com/Netflix/vizceral) to render traffic graph.

#### Features:
- Generates and renders traffic graph in realtime
- Able to replay from any time in the past
- Able to generate notices on node and connection from prom query
- Provides a sidecar application for k8s that watches config changes and reload Vistio server in runtime
- Fits with [Istio](https://istio.io)'s metrics

![](https://github.com/nmnellis/vistio/blob/master/documentation/sample.png)

## Architecture

![](https://github.com/nmnellis/vistio/blob/master/documentation/architecture.png)

## Install

#### Helm chart

If you are using [Helm](https://helm.sh), the simplest way to install is using the charts in `helm` directory with

```
helm install --name vistio ./helm/vistio
```

#### Docker images

Docker images of both `vistio-api` and `vistio-web` are available on Docker Hub.

- [nmnellis/vistio-api](https://hub.docker.com/r/nmnellis/vistio-api)
- [nmnellis/vistio-web](https://hub.docker.com/r/nmnellis/vistio-web)

## Example

An Istio example is in the `/example` directory

You can try it by going to that directory and run

```
docker-compose up --build
```

Then checkout each service at:
- promviz-front: [http://localhost:8080/graph](http://localhost:8080/)
- vistio: [http://localhost:9091/graph](http://localhost:9091/graph)
- prometheus: [http://localhost:9090/graph](http://localhost:9090/graph)
- mock-metric: [http://localhost:30001/metrics](http://localhost:30001/metrics)

## Configuration

See [configuration.md](https://github.com/nmnellis/vistio/blob/master/documentation/configuration.md) in documentation directory.

## Deploy Vistio with Istio Ingress Gateway (kubectl)

1. Deploy Istio
```sh
kubectl apply -f vistio-with-ingress.yaml -n default
```

2. Expose vistio-web
```sh
kubectl -n default port-forward $(kubectl -n default get pod -l app=vistio-web -o jsonpath='{.items[0].metadata.name}') 8080:8080 &
```

3. Open Vistio `localhost:8080`

4. Expose vistio-api
```sh
kubectl -n default port-forward $(kubectl -n default get pod -l app=vistio-api -o jsonpath='{.items[0].metadata.name}') 9091:9091 &
```

5. Test endpoint `localhost:9091/graph`

6. Add traffic to the mesh by following bookinfo demo here https://istio.io/docs/guides/bookinfo/ to get the `GATEWAY_URL` and calling
```sh
curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
```

## Deploy Vistio with Istio Ingress Gateway (helm)

1. Deploy Vistio

```sh
helm install helm/vistio -f helm/vistio/values-mesh-only.yaml --name vistio --namespace default
```

2. Expose vistio-web
```sh
kubectl -n default port-forward $(kubectl -n default get pod -l app=vistio-web -o jsonpath='{.items[0].metadata.name}') 8080:8080 &
```

3. Open Vistio <localhost:8080>

4. Expose vistio-api
```sh
kubectl -n default port-forward $(kubectl -n default get pod -l app=vistio-api -o jsonpath='{.items[0].metadata.name}') 9091:9091 &
```

5. Test endpoint <localhost:9091/graph>

6. Add traffic to the mesh by following bookinfo demo here [Istio Bookinfo Demo](https://istio.io/docs/guides/bookinfo/) to get the `GATEWAY_URL` and calling
```sh
curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
```


## Deploy Vistio Without Istio Ingress (kubectl)

1. Deploy Istio
```sh
kubectl apply -f vistio-mesh-only.yaml -n default
```

2. Expose vistio-web
```sh
kubectl -n default port-forward $(kubectl -n default get pod -l app=vistio-web -o jsonpath='{.items[0].metadata.name}') 8080:8080 &
```

3. Open Vistio `localhost:8080`

4. Expose vistio-api
```sh
kubectl -n default port-forward $(kubectl -n default get pod -l app=vistio-api -o jsonpath='{.items[0].metadata.name}') 9091:9091 &
```

5. Test endpoint `localhost:9091/graph`

6. Add traffic to the mesh by following bookinfo demo here https://istio.io/docs/guides/bookinfo/ to get the `GATEWAY_URL` and calling
```sh
curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
```


## Deploy Vistio Without Istio Ingress (Helm)


* 

1. Deploy Vistio

```sh
helm install helm/vistio -f helm/vistio/values-mesh-only.yaml --name vistio --namespace default
```

2. Expose vistio-web
```sh
kubectl -n default port-forward $(kubectl -n default get pod -l app=vistio-web -o jsonpath='{.items[0].metadata.name}') 8080:8080 &
```

3. Open Vistio <localhost:8080>

4. Expose vistio-api
```sh
kubectl -n default port-forward $(kubectl -n default get pod -l app=vistio-api -o jsonpath='{.items[0].metadata.name}') 9091:9091 &
```

5. Test endpoint <localhost:9091/graph>

6. Add traffic to the mesh by following bookinfo demo here [Istio Bookinfo Demo](https://istio.io/docs/guides/bookinfo/) to get the `GATEWAY_URL` and calling
```sh
curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage
```
## Contributing

Please feel free to create an issue or pull request.

## LICENSE

Vistio is released under the MIT license. See LICENSE file for details.
