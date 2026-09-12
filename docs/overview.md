# k8s-frontend-backend-troubleshooting-playbook

Playbook de troubleshooting que conecta erros vistos no navegador (F12) com investigação e ações em Kubernetes, Helm e observabilidade (Prometheus/Grafana/Loki/traces).

## Para quem é este projeto?

- Engenheiros de DevOps/SRE que operam clusters Kubernetes.
- Times de plataforma que oferecem Kubernetes como produto interno.
- Engenheiros de observabilidade que querem aproximar frontend e backend nos incidentes.

## O que você vai encontrar aqui

- Explicação curta do que são os “logs” do F12 (Console e Network).
- Playbooks no formato: **X (erro no F12) → Y (o que olhar no backend) → Z (ações em k8s/Helm/observabilidade)**.
- Exemplos de queries (Loki, PromQL) e trechos de `values` de Helm para:
  - CORS
  - Logging estruturado com `requestId`/`traceId`
  - Timeouts e retries em Ingress/service mesh.

## Estrutura do repositório

- [`docs/`](docs/) – Conceitos básicos (F12, observabilidade, correlação de traces).
- [`playbooks/`](playbooks/) – Playbooks de troubleshooting por tipo de erro.
- [`examples/`](examples/) – Queries e configurações de exemplo (Loki, PromQL, Helm).
- [`assets/`](assets/) – Diagramas e imagens (se quiser incluir no futuro).

## Playbooks disponíveis

| Playbook | Sintoma principal (F12) |
|---------|--------------------------|
| [js-error-console.md](playbooks/js-error-console.md) | Erro de JavaScript no Console (`TypeError`, `ReferenceError`, etc.) |
| [http-4xx.md](playbooks/http-4xx.md) | Requisições com status 400, 401, 403, 404 |
| [http-5xx.md](playbooks/http-5xx.md) | Requisições com status 500, 502, 503, 504 |
| [cors-error.md](playbooks/cors-error.md) | Erro de CORS no Console / requisições bloqueadas |
| [request-timeout.md](playbooks/request-timeout.md) | Requisições “penduradas” ou com timeout |
| [tls-ssl-errors.md](playbooks/tls-ssl-errors.md) | Erros de certificado, HTTPS e mixed content |
| [request-trace-correlation.md](playbooks/request-trace-correlation.md) | Como usar `requestId`/`traceId` para correlacionar F12 e logs/traces |

## Como usar

1. Identifique o sintoma no navegador (aba Console ou Network do F12).
2. Abra o playbook correspondente em [`playbooks/`](playbooks/).
3. Siga os passos:
   - O que procurar nos logs do backend.
   - Quais queries usar em Loki/Grafana/Prometheus.
   - Quais ações típicas em Kubernetes/Helm (checks, ajustes, rollbacks).

## Exemplo rápido de uso

**Sintoma:**  
- No F12 → Network: `POST /api/users` com status `500` e resposta:
  ```json
  { "error": "Internal server error", "traceId": "abc123" }
  ```

**Playbook:**  
- [`playbooks/http-5xx.md`](playbooks/http-5xx.md)

**Ações típicas:**

- No Loki:
  ```logql
  {app="api"} | json | status=~"5.." | path="/api/users"
  ```
- Buscar por `traceId`:
  ```logql
  {app="api"} | json | traceId="abc123"
  ```
- No cluster:
  ```bash
  kubectl get deploy api -n <ns> -o yaml
  kubectl describe pod -n <ns> -l app=api
  kubectl logs -n <ns> -l app=api --tail=200
  ```

## Contribuindo

Contribuições são bem-vindas: novos playbooks, melhorias em queries, exemplos para outras stacks (Go, .NET, etc.).

- Abra issues com ideias de novos cenários.
- Envie PRs com:
  - Novo playbook em `playbooks/`.
  - Atualizações em queries e exemplos.

Veja mais detalhes em [`CONTRIBUTING.md`](CONTRIBUTING.md).

## Licença

Este projeto está sob a licença MIT. Veja o arquivo [`LICENSE`](LICENSE) para detalhes.
