---
title: "Gems & Landi: Série de Documentação de AWS Cloud Labs"
summary: "Um padrão de documentação contínuo e autopublicado para labs práticos de AWS — redes, segurança e serverless — escrito com mais profundidade do que a documentação oficial."
date: 2026-08-23
---

## O que é

Uma série de documentação aprofundada sobre labs práticos de cloud na AWS, mantida em
Markdown/Obsidian e publicada no repositório "The-Forge". Ela existe porque a maioria dos
tutoriais da AWS mostra *que* algo funciona, não *por que* está configurado daquele jeito —
e a distância entre essas duas coisas é onde nascem os incidentes de produção.

## Escopo

A série cobre atualmente labs em:

- **Redes** — decisões de design de VPC, estratégia de subnets, casos extremos de roteamento
- **Segurança** — estrutura de políticas IAM, regras de WAF, o raciocínio por trás de
  configurações de menor privilégio, e não apenas os passos para ativá-las
- **Serverless** — padrões de Lambda, API Gateway e DynamoDB, incluindo os modos de falha que
  só aparecem depois que você passou tráfego real por eles

## Por que a profundidade importa

Venho de um contexto em que SOPs e documentação técnica determinavam diretamente se um
engenheiro de NOC às 3 da manhã conseguia resolver um incidente sem escalar. Escrevo estes
labs da mesma forma: parto do princípio de que o leitor precisa *entender* a decisão, e não
apenas copiar o comando. Esse é o padrão que exijo desta série — e é, francamente, o padrão
que eu gostaria de receber de quem documenta a infraestrutura da qual dependo.

---

*Série completa: [repositório The-Forge](https://github.com/drlandi)*
