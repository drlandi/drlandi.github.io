---
title: "Decisões Tipadas vs. Prosa: Uma Demo de Triagem de Tickets de Suporte"
summary: "Uma demo ao vivo, lado a lado, de duas formas de um modelo de IA triar tickets de suporte — decisões tipadas rápidas versus prosa gerada, mais lenta — construída sobre um contrato de modelo publicado e localizada para o português brasileiro."
date: 2026-09-28
---

## A ideia

A TypeSafe AI descreve seu modelo Jev como um modelo "System One": em vez de gerar prosa,
ele devolve uma decisão tipada, como um reflexo, enquanto um LLM convencional delibera em
texto livre. O enquadramento vem do pensamento rápido e lento de Kahneman. Eu queria ver
como essa diferença se manifesta em um fluxo de trabalho concreto, então construí uma demo
de triagem de tickets de suporte que coloca as duas abordagens em corrida.

## O que construí

- Uma corrida de triagem lado a lado: os mesmos tickets passam por um caminho de decisão
  tipada e por um caminho de geração de prosa, de modo que a diferença de formato e de
  velocidade fica visível em vez de apenas descrita.
- O lado tipado segue o contrato de entrada e saída publicado do Jev. A API em si está em
  beta fechado, então a demo simula o contrato localmente, e o ponto de chamada está
  preparado para que a simulação seja trocada pela requisição real quando o acesso abrir.
- Duas versões publicadas: inglês e português brasileiro, esta localizada e não apenas
  traduzida — os tickets, os rótulos e o fluxo foram adaptados ao contexto brasileiro.

**Experimente:** [demo em inglês](https://singular-kashata-ca802b.netlify.app/) ·
[Versão em português](https://jovial-mooncake-a2f140.netlify.app/)

## O que a demo mostra e o que não mostra

Como o modelo tipado é simulado, a demo mostra o padrão de interação e o contrato, não
números reais de latência ou acurácia. Esse é o limite honesto do trabalho. O padrão, ainda
assim, é a parte interessante: uma decisão devolvida como valor tipado pode ser validada,
roteada e testada como qualquer outro dado, enquanto texto livre precisa ser interpretado e
confiado antes.

Quando o acesso à API abrir, vou trocar pela chamada real, medir latência e concordância em
relação ao caminho de prosa e publicar os resultados aqui.

## Por que isso importa para mim

Boa parte da minha carreira foi em sistemas nos quais um componente imprevisível era um
risco. Restringir o que um modelo de IA pode devolver é o mesmo instinto aplicado a um novo
tipo de componente: tornar a interface rígida o bastante para que o resto do sistema possa
confiar nela.
