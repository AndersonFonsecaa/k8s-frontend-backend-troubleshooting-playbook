# Exemplos de PromQL

## Erro rate de requisições HTTP (5xx)

```promql
sum(rate(http_requests_total{status=~"5.."}[5m])) by (service)
```

## Latência média por serviço

```promql
sum(rate(http_request_duration_seconds_sum[5m])) by (service)
/
sum(rate(http_request_duration_seconds_count[5m])) by (service)
```

## Uso de CPU por pod

```promql
sum by (pod) (
  rate(container_cpu_usage_seconds_total{namespace="prod"}[5m])
)
```

## Restart de pods

```promql
increase(kube_pod_container_status_restarts_total{namespace="prod"}[1h])
```
