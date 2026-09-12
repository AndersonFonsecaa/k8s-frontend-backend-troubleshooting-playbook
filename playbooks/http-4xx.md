# Requisições com erro 4xx (400, 401, 403, 404)

## X – O que você vê no F12

- Aba **Network**:
  - Requisição `GET/POST/PUT/DELETE /api/...` em vermelho.
  - Status: `400`, `401`, `403` ou `404`.
  - Payload e resposta visíveis (ex.: `{ "error": "Unauthorized" }`).

## Y – O que procurar no backend / observabilidade

- Logs da API:
  - Filtrar por `status =~ "4.."` ou `level="warn"/"error"`.
  - Usar `requestId` / `traceId` se disponível.
- Exemplo Loki:
  ```logql
  {app="api"} | json | status=~"4.." | path="/api/users"
  ```

## Z – Ações típicas em Kubernetes / Helm / observabilidade

- **400 Bad Request:**
  - Validar se o schema esperado mudou (nova versão da API).
  - Checar logs de validação (`validation_failed`, etc.).
  - Ajustar contrato frontend ↔ backend ou fazer rollback da versão da API.
- **401/403 (auth/authorization):**
  - Verificar logs de auth (token inválido, expirado, escopo insuficiente).
  - Checar configuração de:
    - Ingress (auth middleware, OAuth2 proxy, etc.).
    - Service mesh (Istio/Linkerd policies).
  - Ajustar políticas de acesso ou renew de tokens/credenciais.
- **404 Not Found:**
  - Confirmar se a rota existe no serviço.
  - Checar se o deploy atual realmente contém essa rota.
  - Validar configurações de Ingress/Service (path rules, rewrite, etc.).
