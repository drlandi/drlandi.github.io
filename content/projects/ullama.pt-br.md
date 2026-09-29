---
title: "uLLAMA: Um Runtime Mínimo em C para Inferência Local de LLMs"
summary: "Um pequeno wrapper em C sobre o llama.cpp que carrega modelos GGUF e roda uma sessão interativa com medição de tokens por segundo — feito para caber em um notebook de 8 GB, só com CPU, e escrito sob a Norma da 42."
date: 2026-09-29
---

## Por que construir

Eu queria ser dono do loop de inferência em vez de tratá-lo como uma caixa-preta. A
restrição foi deliberada: um notebook de 8 GB, só com CPU, sem GPU. Quando memória e
processamento são tão limitados, cada decisão — tamanho de contexto, número de threads,
estratégia de amostragem, quantização do modelo — aparece imediatamente no comportamento do
modelo, o que é uma ótima forma de aprender o que realmente acontece entre um prompt e uma
resposta.

Também é escrito na mesma disciplina de C que pratico na 42 São Paulo: a Norma da 42,
gerenciamento manual de recursos e nenhum framework entre mim e o modelo.

## O que faz hoje

- Carrega um modelo GGUF local pela API em C do llama.cpp e inicializa um contexto de 2048
  tokens rodando em quatro threads.
- Monta uma cadeia de amostragem — top-k 40, top-p 0.9, temperatura 0.7 — para que a geração
  seja controlada e reproduzível, em vez de deixada nos valores padrão.
- Executa uma sessão interativa: tokeniza o prompt, decodifica um token por vez, exibe a
  saída conforme é produzida e imprime os tokens por segundo após cada resposta.
- Libera o sampler, o contexto e o modelo em ordem na saída, e silencia os logs do backend
  para que o terminal mostre apenas a conversa.

O núcleo tem cerca de cem linhas de C. É intencional: pequeno o bastante para ser lido de
uma vez e compreendido por completo.

## Para onde vai

O repositório está preparado para as próximas etapas, que são planejadas e ainda não estão
concluídas:

- **Fine-tuning específico de domínio** — uma pasta de datasets para dados de treino em
  JSONL (normas de C da 42, Python, fluxos de trabalho AWS) e um espaço para notebook de
  treino de modelos pequenos com Unsloth e exportação para GGUF.
- **Uma fábrica de código offline** — um harness que usa o binário compilado para gerar e
  testar código sem depender de rede, de modo que um modelo local pequeno faça o trabalho
  barato e de alto volume, e os modelos maiores fiquem reservados para os problemas difíceis.

Vou documentar cada etapa aqui conforme ela se tornar real, incluindo os números, para que
os textos mostrem o que funcionou e o que não funcionou.

---

*Código: [github.com/drlandi/ullama](https://github.com/drlandi/ullama)*
