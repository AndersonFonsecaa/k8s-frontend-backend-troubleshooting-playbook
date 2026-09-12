# Correlação usando `requestId` / `traceId`

## X – O que você vê no F12

- Aba **Network**, em uma requisição:
  - Headers da resposta com:
    - `X-Request-Id: <algum-uuid>`
  - Ou resposta JSON com:
    ```json
    { "traceId": "abc123" }
    ```

## Y – O que procurar no backend / observabilidade

- Filtrar logs por esse ID:
  - Loki:
    ```logql
    {app="api"} | json | requestId="abc123"
    ```
  - ELK:
    ```kql
    requestId: "abc123"
    ```
- No tracing (Jaeger/Tempo):
  - Buscar por `traceId` para ver o fluxo completo entre serviços.

## Z – Ações típicas em Kubernetes / Helm / observabilidade

- Garantir que:
  - A aplicação propague `X-Request-Id` / headers de trace entre serviços.
  - Os logs estejam em formato estruturado (JSON) com campos `requestId`, `traceId`, `spanId`.
- Em Helm:
  - Habilitar middlewares de request logging e tracing nos charts (ex.: OpenTelemetry, filtros de log).
- Em dashboards:
  - Criar painéis que permitam colar um `requestId` e ver todos os logs e traces daquela requisição.
