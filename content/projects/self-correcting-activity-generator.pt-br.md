---
title: "Gerador de Atividades Pedagógicas com Autocorreção por IA"
summary: "Uma aplicação web construída em 48 horas para o Hackathon IBM Bob 2.0 que gera atividades práticas e editáveis pelo professor para qualquer nível de recursos — com uma segunda passada de IA que audita a própria saída em seis verificações de qualidade antes que o professor a veja."
date: 2026-09-27
weight: 1
---

## O problema

Pede-se aos professores aulas práticas e relevantes para o contexto local, mas planos de aula
genéricos supõem um nível de recursos que a sala pode não ter: um laboratório com GPUs em uma
escola, uma sala com lousa e sem material impresso em outra. Eu queria uma ferramenta cuja
saída se adapte à sala de aula, e não o contrário — e em que o professor possa confiar o
bastante para usar, porque um plano de aula que parece correto mas traz um fato errado ou um
cronograma que não fecha é pior do que nenhum.

## O que faz

O professor informa uma disciplina e um tema, além de conexões interdisciplinares e contexto
opcionais (materiais disponíveis, realidade da turma). A aplicação devolve uma atividade com
estrutura pedagógica fixa — gancho narrativo, fluxograma, cronograma, materiais, passo a
passo, perguntas de reflexão e rubrica de avaliação — mais zonas de personalização marcadas
para foto, nota do professor ou folha de atividades do aluno, para que o professor possa
adaptá-la à sua realidade.

Três decisões de projeto moldam a saída:

- **Materiais conforme os recursos.** Em matemática, ciências e engenharia, o prompt prefere
  abordagens práticas e táteis e calibra os materiais ao contexto informado — kits onde a
  escola os tem; canudos, fita e papel recortado onde não tem — e exige que a seção de
  Materiais justifique a escolha.
- **Checagem da premissa.** Se um tema se apoia em uma premissa falsa ou pseudocientífica, a
  aplicação reformula a atividade como um exercício de pensamento crítico, em vez de ensinar a
  afirmação como verdadeira. Temas difíceis, mas legítimos, como atrocidades documentadas ou
  discriminação, são tratados como assuntos normais.
- **Fluxograma imprimível.** O fluxograma em Mermaid vai para uma página própria, que pode ser
  impressa ou projetada separadamente.

## Como funciona

1. Um servidor Express recebe o formulário e monta um prompt estruturado.
2. O Node dispara a CLI do IBM Bob como processo filho (`bob run` com saída em JSON e limite de
   cinco turnos). A resposta é JSON delimitado por linhas, lido linha a linha até encontrar a
   mensagem de resultado.
3. O rascunho passa por uma segunda chamada ao Bob: a **passada de autocrítica**, descrita
   abaixo.
4. A aplicação extrai a atividade final entre duas linhas-marcadoras, separa o fluxograma para
   a página própria, guarda o resultado sob um UUID aleatório e redireciona para ele.

O tratamento de falhas fez parte do projeto desde o início. A geração e a crítica têm timeouts
próprios (60 e 90 segundos). Se a etapa de crítica falhar por qualquer motivo — timeout,
marcadores ausentes, saída malformada —, a aplicação volta à atividade original em vez de não
mostrar nada. Uma resposta sem mensagem de resultado é tratada como recusa de conteúdo e recebe
uma página dedicada, em vez de um stack trace.

## A passada de autocrítica

A segunda passada não é "melhore isto". É um revisor rigoroso com seis verificações
específicas, tiradas de uma auditoria de qualidade de atividades geradas anteriormente:

1. **Fontes de dados** — números contestados apresentados com falsa precisão recebem uma
   ressalva ou um aviso "PROFESSOR: verifique".
2. **Falsas dicotomias** — uma rubrica ou estrutura de papéis que divide uma questão real em
   dois campos opostos é sinalizada onde ela é imposta, e não em uma nota geral em outro ponto.
3. **Escopo versus cronograma** — cinco fases substanciais em um único período de 50 minutos
   são reduzidas ou distribuídas em vários dias.
4. **Rubricas sem exemplos** — cada nível precisa de um indicador observável.
5. **Sem alternativa para apresentação pública** — a alternativa precisa estar embutida na
   própria atribuição da tarefa, e não escondida em um apêndice.
6. **Consistência interna** — o modelo primeiro extrai, por escrito, todas as afirmações de
   todas as seções e só então as compara, de modo que as contradições sejam encontradas por
   comparação explícita, e não por impressão geral.

Duas regras atravessam as seis. Primeiro, **nunca inventar uma citação**: uma referência
fabricada que parece autoritativa é um defeito pior do que um número sem fonte, porque o
professor vai confiar nela e repeti-la. Onde a correção pediria uma fonte, o revisor acrescenta
uma ressalva ou um aviso de verificação. Segundo, **correções devem ser completas**: se uma
mudança toca uma fase cronometrada, todos os outros lugares que a mencionam, e o tempo total,
precisam continuar fechando.

O repositório guarda as sucessivas versões do prompt de crítica e os rascunhos de teste que
usei para ajustá-lo.

## Integração e implantação

O Bob não tem endpoint HTTP público de inferência, então a aplicação chama a CLI. Minha
primeira versão usava `execFile` e travava, esperando pipes de saída que nunca fechavam; trocar
por `spawn`, com stdin ignorado e os fluxos de saída coletados, resolveu.

A CLI é instalada por um script instalador da IBM, e não pelo registro do npm; portanto, um
`npm install` comum em um host de nuvem não a fornece. O Dockerfile parte de `node:22-slim`,
executa o instalador de forma não interativa e roda `bob --version` como etapa de build, para
que uma instalação quebrada falhe no build e não em tempo de execução. O contêiner foi
publicado no Railway durante o hackathon, com a chave da API fornecida por variável de ambiente.

## Limites

- As atividades ficam em memória, indexadas por UUID, o que bastou para uma demo de 48 horas.
  Elas somem em uma reinicialização; uma implantação real precisa de armazenamento persistente.
- A crítica é outra chamada ao modelo. Ela reduz classes específicas de defeitos, mas não prova
  correção, e por isso deixa avisos "PROFESSOR: verifique" em vez de chutar. O professor
  continua sendo o revisor final.
- Um script independente, `validate-activity.js`, verifica se as atividades geradas contêm as
  seções obrigatórias. Usei-o durante os testes, mas ele não está integrado à interface web.
- A interface está, por enquanto, apenas em inglês.

---

*[Código](https://github.com/drlandi/ibm-bob-activity-generator)*

A demo pública ficou no ar no Railway durante e depois do hackathon. Ela dependia da chave de API do hackathon e não está mais em execução.
