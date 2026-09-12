# Básico sobre o F12 (DevTools do navegador)

Quando você pressiona **F12** no navegador, abre-se o painel das **Ferramentas do Desenvolvedor** (DevTools). As abas mais relevantes para troubleshooting são:

## Console

- Mostra:
  - Erros de JavaScript (`TypeError`, `ReferenceError`, etc.).
  - Avisos (depreciações, problemas de performance, etc.).
  - Logs manuais inseridos no código (`console.log`, `console.warn`, `console.error`).
- Útil para:
  - Identificar bugs de frontend.
  - Entender se o código chegou a disparar (ou não) uma requisição ao backend.

## Network (Rede)

- Registra todas as requisições HTTP/HTTPS feitas pela página:
  - Chamadas a APIs (fetch, XMLHttpRequest).
  - Carregamento de imagens, CSS, JS, fontes, etc.
- Para cada requisição, é possível ver:
  - URL, método (GET/POST...), status (200, 404, 500...).
  - Headers de request e response.
  - Payload (dados enviados) e resposta (JSON, HTML, etc.).
  - Timings (tempo de DNS, TCP, TTFB, download).

Essas informações são a base para correlacionar erros de frontend com logs e métricas do backend.
