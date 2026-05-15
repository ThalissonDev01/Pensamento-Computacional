# Gestão Financeira para Pequenas e Médias Empresas

> **Atividade Prática — Do caos à organização**
> Representação do mesmo conjunto de dados em três estruturas diferentes: Lista, Hierarquia e Grafo.

---

## Integrantes do Grupo

Davi Pereira dos Santos: https://github.com/Saigaton
Eduardo Roberto Lucena: https://github.com/Bigodudys
Thalisson Costa Mesquita: https://github.com/ThalissonDev01
---

## Sobre a atividade

O objetivo desta atividade é demonstrar que um mesmo conjunto de informações do mundo real pode ser modelado de formas completamente diferentes, dependendo da perspectiva ou necessidade de quem o analisa.

O tema escolhido foi **Gestão Financeira para PMEs** — um conjunto rico de elementos interligados que se presta bem às três estruturas exigidas, pois seus componentes têm ao mesmo tempo uma sequência lógica (Lista), uma relação de autoridade e responsabilidade (Hierarquia) e dependências mútuas (Grafo).

---

## Conjunto de dados

O conjunto de informações utilizado representa as **seis áreas centrais da gestão financeira** de uma pequena ou média empresa:

| # | Elemento | Descrição |
|---|----------|-----------|
| 1 | Fluxo de Caixa | Controle diário de entradas e saídas financeiras |
| 2 | Contas a Pagar e a Receber | Gestão de obrigações e créditos pendentes |
| 3 | DRE | Demonstrativo de Resultado do Exercício |
| 4 | Planejamento Orçamentário | Projeção e controle de receitas e despesas |
| 5 | Indicadores Financeiros (KPIs) | Métricas de desempenho: margem, liquidez, ROI |
| 6 | Captação de Crédito | Linhas de crédito, BNDES e capital de giro |

---

## Estruturas desenvolvidas

### 01 — Lista

**Arquivo:** `Lista.md`

A Lista organiza os elementos em uma **sequência linear e progressiva**, do mais operacional ao mais estratégico. Cada etapa depende da anterior: sem controlar o caixa, não há como interpretar o DRE; sem o DRE, não há base para o planejamento orçamentário.

**Regra estrutural respeitada:** ordenação com índice único por elemento, sem ramificações, sem relações cruzadas — apenas uma direção.

---

### 02 — Grafo

**Arquivo:** `Grafo.pdf`

O Grafo representa as **conexões e dependências mútuas** entre os elementos — sem hierarquia, sem sequência obrigatória. O Fluxo de Caixa é o nó central porque todos os outros elementos o afetam ou dependem dele.

**Regra estrutural respeitada:** nós conectados por arestas direcionadas (relação direta) ou não-direcionadas (relação bidirecional), podendo haver múltiplas conexões entre qualquer par de nós.

---

### 03 — Hierarquia

**Arquivo:** `Hierarquia.pdf`

A Hierarquia estrutura os elementos por **nível de autoridade e responsabilidade**, mostrando quem decide o quê dentro da gestão financeira de uma PME.

**Regra estrutural respeitada:** cada nó tem exatamente um pai (exceto a raiz), formando uma árvore sem ciclos e sem conexões laterais.

---

## Critérios de sucesso atendidos

| Critério | Como foi atendido |
|----------|-------------------|
| **Clareza Visual** | Cada arquivo usa estrutura clara e visual para facilitar a leitura imediata |
| **Correção Estrutural** | Lista respeita ordenação linear; Hierarquia forma uma árvore sem ciclos; Grafo usa nós e arestas com direção |
| **Criatividade na Escolha** | Gestão Financeira para PMEs é um tema do mundo real, com elementos ricos o suficiente para as três estruturas |
| **Organização Técnica** | Arquivos nomeados conforme padronização exigida, README documentando o projeto |

---

## Estrutura dos arquivos

```
Aula_Organizacao_Dados_GrupoX/
├── README.md           ← Explicação da atividade e objetivos
├── Lista.md            ← Representação linear
├── Grafo.pdf           ← O mapa de conexões
└── Hierarquia.pdf      ← O organograma visual
```

---

## Conclusão

A principal lição desta atividade é que **não existe uma única forma correta de representar dados** — a escolha da estrutura depende do que se quer comunicar:

- Use uma **Lista** quando a ordem importa e os elementos têm uma progressão natural.
- Use uma **Hierarquia** quando existem relações de autoridade, composição ou subordinação.
- Use um **Grafo** quando os elementos têm múltiplas relações entre si e nenhuma direção única faz sentido.

No caso da gestão financeira de uma PME, as três perspectivas são válidas e complementares — cada uma revela um aspecto diferente do mesmo conjunto de informações.
