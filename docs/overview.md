# Visão geral do projeto

Este repositório reúne playbooks de troubleshooting que ligam:

- O que o engenheiro vê no navegador (F12 → Console e Network).
- O que acontece no backend rodando em Kubernetes.
- Como investigar e agir usando ferramentas de observabilidade (Prometheus, Grafana, Loki, traces).

A ideia é ter um guia prático, em formato de checklist, para acelerar a investigação de incidentes que envolvem frontend e backend.

## Motivação

Em muitos incidentes:

- O frontend reporta um erro genérico (“algo deu errado”).
- O backend tem logs, métricas e traces, mas ninguém sabe por onde começar.
- A correlação entre o que o usuário vê e o que os sistemas registram é manual e lenta.

Este projeto propõe um caminho estruturado:

1. Identificar o sintoma no F12.
2. Seguir um playbook específico para aquele sintoma.
3. Usar queries e comandos prontos para investigar no cluster e nas ferramentas de observabilidade.

## Público-alvo

- SREs e engenheiros de DevOps que operam Kubernetes.
- Engenheiros de observabilidade.
- Desenvolvedores full-stack que querem entender melhor o ciclo completo do erro.
