- Istio configuration

```
docker-compose up --build
```

Now, you can reach each service at

- vistio: [http://localhost:9091/graph](http://localhost:9091/graph)
- prometheus: [http://localhost:9090/graph](http://localhost:9090/graph)
- mock-metric: [http://localhost:30001/metrics](http://localhost:30001/metrics)