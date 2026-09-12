# Erro de JavaScript no Console (TypeError, ReferenceError, etc.)

## X – O que você vê no F12

- Aba **Console**:
  - Mensagens como:
    - `Uncaught TypeError: Cannot read properties of null...`
    - `ReferenceError: x is not defined`
  - Stack trace indicando arquivo JS e linha.

## Y – O que procurar no backend / observabilidade

- Muitas vezes **nenhum log** relacionado, porque a requisição nem saiu do navegador.
- Se houver logs, serão de requisições anteriores (ex.: carregamento da página).

## Z – Ações típicas em Kubernetes / Helm / observabilidade

1. Confirmar que **não há erro 4xx/5xx** na aba Network para essa ação.
2. Se não houver chamada ao backend:
   - Acionar time de frontend / revisar deploy do frontend:
     - `kubectl get deploy frontend -n <ns> -o yaml`
     - Conferir `image:` e `tag`.
   - Verificar se a versão em produção é a esperada (tag do chart, commit, etc.).
3. Se usar error tracking no cliente (Sentry, etc.):
   - Consultar dashboards de erros de frontend.
   - Correlacionar com versões da aplicação (tag do chart, release).
