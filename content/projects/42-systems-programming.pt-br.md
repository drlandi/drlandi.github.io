---
title: "Programação de Sistemas na 42 São Paulo"
summary: "Reimplementando a biblioteca padrão de C, construindo um leitor de arquivos linha a linha com memória estática e co-desenvolvendo um algoritmo de ordenação de pilhas — as bases que a maioria dos engenheiros de cloud pula."
date: 2026-08-21
---

## Por que isso importa para infraestrutura

A maioria das pessoas chega à engenharia de cloud vinda do desenvolvimento de aplicações ou de
bootcamps. Eu vim na direção oposta — 25 anos operando a infraestrutura sobre a qual o código
dos outros roda — e o currículo da 42 exige exatamente o entendimento de baixo nível que
essa trajetória não demandava até agora: gerenciamento manual de memória, sem garbage
collector para se esconder atrás, sem bibliotecas para tratar como mágica.

## Trabalhos selecionados

**libft** — reimplementei do zero 43 funções centrais da biblioteca padrão de C e depois as
apliquei na construção de um projeto de catálogo de produtos em linha de comando, com
documentação completa. É a base sobre a qual todo o resto do currículo se apoia.

**get_next_line** — um leitor de arquivos linha a linha que usa memória estática entre
chamadas de função. O interessante não foi ler o arquivo, e sim resolver casos extremos de
falha de malloc e dois vazamentos de memória que só apareciam em condições específicas de
limite de buffer.

**push_swap** — co-desenvolvido com um parceiro ([Luciano](https://github.com/lfurtado)):
um projeto de algoritmo de ordenação de pilhas, com estratégias adaptativas ajustadas para
grandes conjuntos de dados, em que a abordagem ingênua estoura o orçamento rapidamente.

**ft_printf** — uma implementação própria de `printf` feita do zero, incluindo o tratamento
completo de argumentos variádicos (`va_list`).

## A conexão com o trabalho em cloud

Nada disso é abstrato para mim. Depurar um vazamento de memória no `get_next_line` e depurar
por que a alocação de memória no cold start de uma função Lambda está se comportando de forma
inesperada são o mesmo tipo de problema em altitudes diferentes. Esse é o argumento que eu
usaria para explicar por que uma base em nível de sistema importa mesmo em um mundo
serverless: as abstrações vazam, e quando isso acontece, é bom ter construído aquilo que elas
abstraem.

---

*Código: [GitHub](https://github.com/drlandi)*
