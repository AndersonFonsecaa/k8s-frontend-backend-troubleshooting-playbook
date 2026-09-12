# Erros de certificado / HTTPS / mixed content

## X – O que você vê no F12

- Aba **Console** / **Security**:
  - Mensagens como:
    - `Mixed Content: The page at 'https://...' was loaded over HTTPS, but requested an insecure resource 'http://...'`
    - `ERR_SSL_VERSION_OR_CIPHER_MISMATCH`
    - `NET::ERR_CERT_AUTHORITY_INVALID`
- Aba **Security**:
  - Alertas de certificado inválido, expirado, emissor não confiável.

## Y – O que procurar no backend / observabilidade

- Logs de Ingress / proxy:
  - Erros de TLS handshake, certificado inválido, SNI incorreto.
- Logs da aplicação:
  - Falhas ao chamar recursos externos por HTTP/HTTPS com certificado inválido.

## Z – Ações típicas em Kubernetes / Helm / observabilidade

- Validar certificados:
  - `kubectl get cert -n <ns>` (se usar cert-manager).
  - Checar expiration e emissor.
- Ajustar Ingress:
  - TLS config, secret de certificado, hosts corretos.
- Corrigir mixed content:
  - Garantir que o frontend use apenas URLs `https://` para recursos.
- Em Helm:
  - Atualizar values de TLS, certificados e URLs de endpoints nos charts.
