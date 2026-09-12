# Exemplos de queries Loki

## Erros 5xx por serviço

```logql
sum by (service) (
  count_over_time({namespace="prod"} | json | status=~"5.." [5m])
)
```

## Logs por `requestId`

```logql
{namespace="prod"} | json | requestId="abc123"
```

## Erros de CORS (se logados)

```logql
{app="api"} | json | path=~"/api/.*" | status=~"4.." |~ "CORS"
```

## Latência p95 por rota

```logql
histogram_quantile(
  0.95,
  sum by (le, path) (
    rate(http_request_duration_seconds_bucket{namespace="prod"}[5m])
  )
)
```
