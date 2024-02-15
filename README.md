# multicluster-observability-stack

This is a test repository. I want to validate what is the [promxy](https://github.com/jacksontj/promxy) behavior when one of its targets is down.

## Test

We build two docker-composes that runs two Prometheus (`cluster-a` and `cluster-b`). They scrapes two sample apps ([go-prometheus-app](https://github.com/victoramsantos/go-prometheus-app)),
which just export some metrics. Moreover, we have one container running Promxy, which is [configured](./promxy/prometheus-promxy-config-ignore-errors.yml) to query both Prometheus.

The [docker-compose-prometheus.yml](./docker-compose-prometheus.yml) run with the `ignore_error` flag as `true` and the [docker-compose-prometheus-ignore-errors.yml](./docker-compose-prometheus-ignore-errors.yml) as `false`. 

## Running 

Run the desired docker-compose. Ex.:

```
docker-compose -f docker-compose-prometheus-ignore-errors.yml up 
```

Access the promxy UI at [http://localhost:8082/](http://localhost:8082) and run the query:

```
sum(static_counter)
```

This is a metric collected by our `go-prometheus-app` which always return `1`, so as we have two Prometheus, this should return `2`.
