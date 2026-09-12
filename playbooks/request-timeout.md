# Requisições “penduradas” ou com timeout

## X – O que você vê no F12

- Aba **Network**:
  - Requisição aparece como `(pending)` por muito tempo.
  - Ou falha com:
    - `(failed)`, `ERR_CONNECTION_TIMED_OUT`, `ERR_ABORTED`, etc.
  - Sem status HTTP definido ou demora anormal.

## Y – O que procurar no backend / observabilidade

- Verificar se a requisição chegou ao serviço:
  - Logs de request com `path` e `method`, mas sem log de response.
- Checar logs de proxy/Ingress/service mesh:
  - Timeouts, upstreams indisponíveis, circuit breaker aberto.
- Métricas:
  - Aumento de latência, filas, conexões ativas, saturação de recursos.

## Z – Ações típicas em Kubernetes / Helm / observabilidade

1. Verificar saúde dos pods:
   ```bash
   kubectl get pods -n <ns> -l app=api
   kubectl top pods -n <ns>
   ```
2. Checar timeouts configurados:
   - Ingress (annotations de timeout).
   - Service mesh (timeout/retry no `VirtualService`, `DestinationRule`).
   - Aplicação (timeouts de DB, chamadas externas).
3. Ajustar:
   - `resources` (requests/limits) no Deployment.
   - `readinessProbe` / `livenessProbe`.
   - Políticas de retry/timeout no Helm chart do Ingress ou service mesh.
