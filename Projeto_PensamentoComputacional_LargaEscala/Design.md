# Design.md — ATLAS FinTech
---

## 1. Visão Geral do Design

O ATLAS FinTech é uma aplicação web **SaaS multi-tenant** de gestão financeira empresarial. Seu design foi concebido a partir dos quatro pilares do pensamento computacional — decomposição, reconhecimento de padrões, abstração e algoritmos — e organizado em uma arquitetura orientada a módulos de serviço independentes.

A proposta central é oferecer às empresas uma visão unificada de sua saúde financeira: do lançamento de uma transação até a geração de um relatório gerencial ou a consulta ao assistente de IA, tudo dentro de um único produto coeso.

---

## 2. Decomposição do Sistema

O sistema foi decomposto em **11 módulos de serviço**, cada um com responsabilidade única e bem delimitada:

| # | Módulo | Responsabilidade |
|---|--------|-----------------|
| 1 | **Auth** | Cadastro, login (e-mail/senha e Google OAuth), emissão e renovação de JWT, reset de senha |
| 2 | **Usuários** | Perfil do usuário, preferências, permissões e vínculo com empresas |
| 3 | **Empresas** | Cadastro de empresa (CNPJ, razão social), configurações, isolamento multi-tenant |
| 4 | **Contas Bancárias** | Registro de contas, saldos iniciais, conciliação e movimentações |
| 5 | **Transações** | Lançamentos financeiros (receitas e despesas), categorização e status |
| 6 | **Categorias** | Criação e gestão de categorias de receita/despesa por empresa |
| 7 | **Contas a Pagar / Receber** | Cadastro de obrigações com vencimento, parcelamento e vinculação de transações |
| 8 | **Fluxo de Caixa** | Cálculo de saldo em tempo real, projeção de fim de mês e histórico mensal |
| 9 | **Analytics** | Indicadores financeiros (receita, despesa, lucro, margem), alertas e auditoria |
| 10 | **Relatórios** | Geração e exportação de arquivos CSV, PDF e ZIP; envio por e-mail agendado |
| 11 | **Assistente IA** | Chatbot financeiro com acesso a dados em tempo real via intenções pré-definidas |

### 2.1 Diagrama de Dependências entre Módulos

```
┌─────────┐     ┌──────────┐     ┌───────────┐
│  Auth   │────▶│ Usuários │────▶│ Empresas  │
└─────────┘     └──────────┘     └─────┬─────┘
                                        │
              ┌─────────────────────────┼──────────────────────┐
              ▼                         ▼                       ▼
     ┌────────────────┐       ┌──────────────────┐    ┌─────────────┐
     │ Contas Banc.   │       │   Transações     │    │ Categorias  │
     └───────┬────────┘       └────────┬─────────┘    └──────┬──────┘
             │                         │                      │
             └──────────┬──────────────┘◀─────────────────────┘
                        ▼
            ┌───────────────────────┐
            │  Contas a Pagar /     │
            │     a Receber         │
            └───────────┬───────────┘
                        │
          ┌─────────────┼──────────────┐
          ▼             ▼              ▼
  ┌──────────────┐ ┌──────────┐ ┌──────────────┐
  │ Fluxo de     │ │Analytics │ │  Relatórios  │
  │   Caixa      │ │          │ │              │
  └──────────────┘ └────┬─────┘ └──────────────┘
                        │
                        ▼
                ┌───────────────┐
                │ Assistente IA │
                └───────────────┘
```

---

## 3. Reconhecimento de Padrões

O design do ATLAS FinTech foi informado pela análise de sistemas financeiros consolidados, identificando padrões recorrentes que foram adaptados ao contexto do produto.

### 3.1 Padrões de Interface e Experiência

| Padrão identificado | Referência | Aplicação no ATLAS |
|---------------------|------------|-------------------|
| Sidebar fixa com seções colapsáveis | Conta Azul, Omie | Navegação lateral com grupos Principal / Financeiro / Análise / IA |
| Dashboard com cards de KPI no topo | Power BI, Metabase | Cards de Receita, Despesa, Lucro e Saldo com variação percentual |
| Tabela de lançamentos com status por badge | Omie, QuickBooks | Listagem de transações com badges Confirmado / Pendente / Vencido |
| Gráfico de barras agrupadas por período | Metabase, Conta Azul | Fluxo de caixa mensal com entradas (verde) e saídas (vermelho) |
| Donut chart para composição de despesas | Power BI, Tableau | Distribuição de despesas por categoria na tela Dashboard |

### 3.2 Padrões de Dados e Domínio

| Padrão identificado | Referência | Aplicação no ATLAS |
|---------------------|------------|-------------------|
| Entidade Empresa como raiz de isolamento | SAP, Totvs | Todos os dados vinculados ao `company_id`; nenhum dado vaza entre empresas |
| Plano de contas com categorias hierárquicas | ERPs em geral | Categorias de receita/despesa criadas por empresa, usadas em transações e relatórios |
| Parcelamento gera N transações filhas | Conta Azul, Omie | Conta a pagar parcelada em N vezes cria N registros vinculados ao `parent_id` |
| Conciliação bancária por matching de valores | Sistemas bancários | Comparação entre extrato importado e lançamentos internos por data e valor |

### 3.3 Padrões de Segurança

| Padrão | Princípio | Implementação |
|--------|-----------|---------------|
| Autenticação stateless | JWT (RFC 7519) | Access token (15 min) + Refresh token (7 dias) |
| Menor privilégio | Saltzer & Schroeder | Cada usuário acessa apenas os dados de sua empresa |
| Bloqueio por padrão | Saltzer & Schroeder | Rotas protegidas por padrão; acesso público é exceção explícita |
| Mediação completa | Saltzer & Schroeder | Todo request autenticado valida o `company_id` do token antes de consultar dados |

---

## 4. Abstração

### 4.1 Modelo de Dados — Entidades Centrais

Abaixo estão as entidades centrais do sistema e seus atributos essenciais. Relacionamentos secundários (logs, auditoria, tokens) foram omitidos para clareza.

```
┌──────────────────────┐        ┌──────────────────────────┐
│       User           │        │        Company           │
├──────────────────────┤        ├──────────────────────────┤
│ id (UUID)            │        │ id (UUID)                │
│ name                 │◀──────▶│ name                     │
│ email                │  N:N   │ cnpj                     │
│ password_hash        │        │ plan                     │
│ google_id (opt)      │        │ created_at               │
│ created_at           │        └──────────────────────────┘
└──────────────────────┘                    │ 1
                                            │
              ┌─────────────────────────────┼─────────────────────────────┐
              │                             │                             │
              ▼ N                           ▼ N                           ▼ N
┌─────────────────────┐      ┌──────────────────────┐      ┌──────────────────────┐
│   BankAccount       │      │      Category        │      │    Transaction       │
├─────────────────────┤      ├──────────────────────┤      ├──────────────────────┤
│ id (UUID)           │      │ id (UUID)            │      │ id (UUID)            │
│ company_id (FK)     │      │ company_id (FK)      │      │ company_id (FK)      │
│ name                │      │ name                 │      │ bank_account_id (FK) │
│ bank_name           │      │ type (income/expense)│      │ category_id (FK)     │
│ initial_balance     │      │ color                │      │ description          │
│ current_balance     │      │ created_at           │      │ amount               │
│ created_at          │      └──────────────────────┘      │ type (in/out)        │
└─────────────────────┘                                     │ date                 │
                                                            │ status               │
                                                            │ payable_id (FK, opt) │
                                                            │ created_at           │
                                                            └──────────────────────┘
              ┌──────────────────────────────────────────────────┘
              │
              ▼ N
┌──────────────────────────┐
│    Payable / Receivable  │
├──────────────────────────┤
│ id (UUID)                │
│ company_id (FK)          │
│ description              │
│ total_amount             │
│ installments             │
│ due_date                 │
│ status (pending/paid/...) │
│ parent_id (FK, opt)      │  ◀── parcelamento: aponta para o item pai
│ created_at               │
└──────────────────────────┘
```

### 4.2 Fluxo de Autenticação (Abstrato)

```
Usuário
  │
  ├─ POST /auth/login ──────────────────────────────────────────┐
  │   { email, password }                                        │
  │                                                              ▼
  │                                                   Valida credenciais
  │                                                              │
  │                                              ┌──── Inválido ──▶ 401 Unauthorized
  │                                              │
  │                                         Válido
  │                                              │
  │                                   Emite access_token (15min)
  │                                   Emite refresh_token (7 dias)
  │                                              │
  │◀─────────────────────────────────────────────┘
  │   { access_token, refresh_token }
  │
  ├─ Requisições autenticadas
  │   Authorization: Bearer <access_token>
  │         │
  │         ▼
  │   Middleware valida JWT
  │         │
  │   Extrai { user_id, company_id, role }
  │         │
  │   Passa para o controller
  │
  └─ POST /auth/refresh ────────────────────────────────────────┐
      { refresh_token }                                          │
                                                                 ▼
                                                    Valida e renova tokens
                                                                 │
                                             ◀───────────────────┘
                                          { novo access_token }
```

---

## 5. Algoritmos Principais

### 5.1 Cálculo de Saldo em Tempo Real

```
função calcularSaldo(company_id, account_id, data_referencia):

  saldo_inicial ← BankAccount.initial_balance

  transações ← buscar todas as transações onde:
      company_id = company_id
      bank_account_id = account_id
      date ≤ data_referencia
      status = "confirmado"

  para cada transação em transações:
      se transação.type = "entrada":
          saldo_inicial += transação.amount
      senão:
          saldo_inicial -= transação.amount

  retornar saldo_inicial
```

### 5.2 Projeção de Saldo para Fim do Mês

```
função projetarSaldo(company_id, data_hoje):

  saldo_atual ← calcularSaldo(company_id, todas_contas, data_hoje)

  entradas_previstas ← soma dos receivables onde:
      due_date entre data_hoje e fim_do_mês
      status = "pendente"

  saídas_previstas ← soma dos payables onde:
      due_date entre data_hoje e fim_do_mês
      status = "pendente"

  saldo_projetado ← saldo_atual + entradas_previstas - saídas_previstas

  retornar {
      saldo_atual,
      entradas_previstas,
      saídas_previstas,
      saldo_projetado,
      dias_restantes
  }
```

### 5.3 Parcelamento Automático

```
função criarParcelamento(payable, numero_parcelas):

  parcelas ← []
  valor_parcela ← arredondar(payable.total_amount / numero_parcelas, 2)
  ajuste ← payable.total_amount - (valor_parcela × numero_parcelas)

  para i de 1 até numero_parcelas:
      parcela ← {
          parent_id: payable.id,
          description: payable.description + " (" + i + "/" + numero_parcelas + ")",
          amount: valor_parcela + (se i = numero_parcelas então ajuste senão 0),
          due_date: payable.due_date + (i - 1) meses,
          status: "pendente"
      }
      parcelas.adicionar(parcela)

  persistir todas as parcelas em lote
  retornar parcelas
```

### 5.4 Assistente IA — Resolução de Intenção

```
função resolverIntencao(mensagem_usuario, company_id):

  intenção ← classificar(mensagem_usuario)
  // Intenções suportadas: SALDO, RESUMO_MES, CONTAS_PAGAR,
  //                       ALERTAS, RECEITA, DESPESA, PROJECAO

  escolher intenção:

    caso SALDO:
      dados ← calcularSaldo(company_id, todas_contas, hoje)
      retornar formatar("Seu saldo é R$ {dados}")

    caso RESUMO_MES:
      dados ← buscarKPIs(company_id, mes_atual)
      retornar formatar("Receita: {dados.receita}, Despesa: {dados.despesa}...")

    caso CONTAS_PAGAR:
      dados ← listarPayables(company_id, status=["pendente","vencido"])
      retornar formatar("Você tem {N} contas: {lista}")

    caso ALERTAS:
      vencidas ← listarPayables(company_id, status="vencido")
      retornar formatar("Alertas encontrados: {vencidas}")

    padrão:
      retornar "Não entendi. Tente: saldo, resumo do mês, contas a pagar..."
```

---

## 6. Arquitetura da Aplicação

### 6.1 Visão em Camadas

```
┌─────────────────────────────────────────────────────────────┐
│                        CLIENTE (Browser)                     │
│              HTML + CSS + JavaScript (SPA)                   │
└────────────────────────────┬────────────────────────────────┘
                             │ HTTPS / REST JSON
┌────────────────────────────▼────────────────────────────────┐
│                        API GATEWAY                           │
│         Autenticação JWT · Rate Limiting · CORS              │
└────────────────────────────┬────────────────────────────────┘
                             │
       ┌─────────────────────┼──────────────────────┐
       ▼                     ▼                       ▼
┌────────────┐       ┌──────────────┐       ┌──────────────┐
│  Módulo    │       │   Módulo     │       │   Módulo     │
│   Auth     │       │  Financeiro  │       │  Analytics   │
└────────────┘       └──────────────┘       └──────────────┘
                             │
┌────────────────────────────▼────────────────────────────────┐
│                     BANCO DE DADOS                           │
│              PostgreSQL       │
└─────────────────────────────────────────────────────────────┘
                             │
┌────────────────────────────▼────────────────────────────────┐
│                   SERVIÇOS EXTERNOS                          │
│          Google OAuth 2.0 · SMTP (e-mail) · Storage         │
└─────────────────────────────────────────────────────────────┘
```
---

## 7. Decisões de Design

| Decisão | Alternativa considerada | Justificativa |
|---------|------------------------|---------------|
| JWT stateless | Sessões server-side | Escalabilidade horizontal sem estado compartilhado |
| SPA single-file (protótipo) | Framework SPA (React/Vue) | Rapidez de prototipação; sem dependência de build tools |
| Parcelamento por registros filhos | Campo `installments` na transação | Rastreabilidade individual de cada parcela; permite pagar parcelas avulsas |
| Chatbot por intenções fixas | LLM externo | Sem dependência de API externa; respostas determinísticas e seguras para dados financeiros |

---
