# Básico de observabilidade para este projeto

Observabilidade é a capacidade de entender o estado interno de um sistema a partir dos dados que ele produz: logs, métricas e traces.

## Logs

- Registros de eventos (ex.: requisições HTTP, erros, auditoria).
- Idealmente estruturados (JSON), com campos como:
  - `timestamp`
  - `level` (info, warn, error)
  - `message`
  - `requestId` / `traceId`
  - `path`, `method`, `status`
- Ferramentas comuns: Loki, ELK, Cloud Logging, etc.

## Métricas

- Dados numéricos agregados ao longo do tempo:
  - Requisições por segundo.
  - Latência média/p95/p99.
  - Uso de CPU/memória.
- Ferramentas comuns: Prometheus + Grafana.

## Traces

- Registro do caminho de uma requisição entre vários serviços.
- Cada requisição tem um `traceId`, dividido em `spans` (etapas).
- Ferramentas comuns: Jaeger, Tempo, Zipkin, OpenTelemetry.

## Correlação

- Usar `requestId` / `traceId` para:
  - Partir de um erro no F12 (Network) → encontrar o log exato no backend.
  - Navegar entre logs, métricas e traces de uma mesma requisição.
