# Requisições com erro 5xx (500, 502, 503, 504)

## X – O que você vê no F12

- Aba **Network**:
  - Requisição `GET/POST/PUT/DELETE /api/...` em vermelho.
  - Status: `500`, `502`, `503` ou `504`.
  - Às vezes corpo da resposta:
    ```json
    { "error": "Internal server error", "traceId": "abc123" }
    ```

## Y – O que procurar no backend / observabilidade

- Logs da API:
  - Filtrar por `status =~ "5.."` ou `level="error"`.
  - Usar `requestId` / `traceId` se disponível.
- Exemplo Loki:
  ```logql
  {app="api"} | json | status=~"5.." | path="/api/users"
  ```
- Com `traceId`:
  ```logql
  {app="api"} | json | traceId="abc123"
  ```
- Tracing (Jaeger/Tempo):
  - Buscar pelo `traceId` para ver o fluxo entre serviços.

## Z – Ações típicas em Kubernetes / Helm / observabilidade

1. Verificar saúde dos pods:
   ```bash
   kubectl get pods -n <ns> -l app=api
   kubectl describe pod -n <ns> -l app=api
   kubectl logs -n <ns> -l app=api --tail=200
   ```
2. Checar métricas no Prometheus/Grafana:
   - Aumento de erro rate, latência, saturação de CPU/memória.
   - Exemplo PromQL:
     ```promql
     sum(rate(http_requests_total{service="api", status=~"5.."}[5m])) by (service)
     ```
3. Ajustes comuns:
   - Recursos (requests/limits) no Deployment (via Helm values).
   - Configuração de DB, cache, filas.
   - Rollback da versão do serviço (chart Helm) se o erro começou após deploy.
   - Ajuste de timeouts/retries no Ingress ou service mesh.
