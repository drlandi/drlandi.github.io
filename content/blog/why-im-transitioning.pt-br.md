---
title: "O que 25 anos de operações de telecom me ensinaram sobre infraestrutura cloud"
summary: "Os instintos não mudam quando o hardware muda — só ganham outros nomes."
date: 2026-09-25
---

Por 25 anos mantive no ar coisas que não podiam cair. Implantações de GSM e WCDMA em seis
países, operação de NOC em sistemas Unix de nível carrier, bancos de dados de pré-pago em que
dois minutos de indisponibilidade significavam uma fila de suporte que ninguém queria assumir.
As ferramentas eram Solaris, Nortel, Motorola, Tekelec. A disciplina era ITIL e eTOM. Nada
dessas ferramentas é o que uso hoje.

A disciplina é exatamente a mesma.

## O padrão se repete

Quando sentei para projetar uma arquitetura com failover para um hackathon recente —
CloudFront na frente de dois buckets S3 de origem, um em `sa-east-1` e outro em `us-east-1` —
eu não estava pensando em checklists de boas práticas da AWS. Estava pensando na mesma
pergunta que fiz por duas décadas: *o que acontece quando esta região apaga, e alguém percebe
antes das métricas?*

A resposta, naquela época e agora, é a mesma: desacoplar as partes que podem falhar de forma
independente, tornar o failover automático em vez de procedimental e instrumentar as
junções para que uma pessoa descubra o problema por um dashboard, e não por um telefonema.

## O que realmente é novo

A sintaxe é nova. Lambda no lugar de um runbook de NOC. DynamoDB no lugar de um banco de
pré-pago em HP-UX. CloudWatch no lugar do modelo mental de um engenheiro de NOC de que "essa
métrica costuma ser assim". Terraform no lugar de um ticket de gestão de mudanças. Estou
aprendendo tudo isso de forma deliberada, pelo currículo de programação de sistemas da 42 São
Paulo e construindo arquiteturas AWS reais, em vez de seguir tutoriais.

Mas eu estaria fazendo um desserviço à área se apresentasse isso como começar do zero. O
instinto sobre onde os sistemas realmente quebram — não onde a documentação diz que podem
quebrar, mas onde quebram de fato, às 3 da manhã, sob carga real — não se transfere por uma
certificação. É o que 25 anos me deram, e não estou disposto a descontá-lo só porque a
infraestrutura agora é definida em YAML em vez de instalada em um data center.

## Para onde isso vai

Estou documentando a construção enquanto avanço — write-ups de arquitetura, a série de labs
AWS, o trabalho de programação de sistemas na 42 — porque o valor não está só em ter
construído essas coisas. Está em conseguir explicar *por que* cada decisão foi tomada, que é a
parte que a maioria dos tutoriais de cloud pula e que a maioria dos incidentes em produção
expõe.
