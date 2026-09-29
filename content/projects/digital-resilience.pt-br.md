---
title: "Digital Resilience: Arquitetura Serverless Resiliente a Falhas"
summary: "Uma arquitetura AWS de alta disponibilidade com failover de dupla origem, construída e testada ao vivo para o EDN Hackathon Future Builders (Case B: Digital Resilience)."
date: 2026-09-01
---

## Contexto

Construído de forma independente para o EDN Hackathon "Future Builders" (Escola da Nuvem ×
AWS), Case B: Digital Resilience — um desafio para projetar infraestrutura que permaneça
disponível em condições de falha regional.

## Arquitetura

- **AWS WAF + CloudFront** na frente de uma configuração de dupla origem: S3 primário em
  `sa-east-1` e origem secundária de failover em `us-east-1`
- **API Gateway** com integração direta com SQS — desacoplando a ingestão do processamento
  para que um backlog não vire uma indisponibilidade
- **Worker Lambda** consumindo do SQS e gravando no **DynamoDB**
- **CloudWatch + SNS** para observabilidade e alertas ao longo do caminho de failover

## O que eu realmente testei

Isso não foi um exercício de diagrama. Validei a arquitetura de ponta a ponta com envios
reais de formulário e scans no DynamoDB — confirmando que os dados chegavam corretamente
tanto em condições normais quanto em failover. Também auditei o ambiente de um colega,
provisionado com Terraform, contra os requisitos do projeto, o que revelou lacunas que uma
revisão apenas do diagrama não teria pegado.

## Por que este desenho

Vindo de 25 anos de requisitos de uptime de nível carrier, o instinto aqui não foi "fazer
funcionar" — foi "fazer sobreviver à perda de uma região sem que ninguém perceba". O failover
de dupla origem e o desacoplamento com SQS são o mesmo padrão que eu aplicaria a qualquer
sistema em que a indisponibilidade tem custo real, só que expresso em primitivas nativas da
AWS em vez de infraestrutura de telecom.

---

*Código e diagramas de arquitetura: [GitHub](https://github.com/drlandi)*
