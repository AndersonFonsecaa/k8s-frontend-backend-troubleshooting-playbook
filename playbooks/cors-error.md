# Erro de CORS no Console / requisições bloqueadas

## X – O que você vê no F12

- Aba **Console**:
  - Mensagem como:
    - `Access to fetch at 'https://api...' from origin 'https://app...' has been blocked by CORS policy...`
- Aba **Network**:
  - Requisição `OPTIONS` (preflight) ou a própria requisição em vermelho, muitas vezes sem resposta legível.

## Y – O que procurar no backend / observabilidade

- Logs do serviço/API e/ou Ingress:
  - Requests `OPTIONS` para o endpoint.
  - Erros de configuração de CORS ou headers não enviados.
- Se logar headers:
  - Verificar presença de:
    - `Access-Control-Allow-Origin`
    - `Access-Control-Allow-Methods`
    - `Access-Control-Allow-Headers`

## Z – Ações típicas em Kubernetes / Helm / observabilidade

- Ajustar CORS no backend ou no edge:
  - Aplicação: habilitar CORS para o origin do frontend.
  - Ingress/API Gateway: configurar políticas de CORS.
- Validar com:
  - Aba Network → inspecionar headers da resposta `OPTIONS`.
- Se usar service mesh (Istio, por exemplo):
  - Conferir `VirtualService` / `EnvoyFilter` com regras de CORS.
- Em Helm:
  - Ajustar values de CORS no chart do serviço ou do Ingress e fazer upgrade.
