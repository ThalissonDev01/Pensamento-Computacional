# Projeto – Pensamento Computacional para Sistemas de Larga Escala

## Descrição

Este projeto foi desenvolvido como parte da disciplina Pensamento Computacional, com a Profa. Kadidja Valéria.
O objetivo é aplicar os conceitos de pensamento computacional e engenharia de software na concepção de um sistema de larga escala, explorando decomposição, abstração, reconhecimento de padrões e algoritmos.

---

## Objetivos

- Relacionar engenharia de software e pensamento computacional.
- Reconhecer princípios e padrões relevantes para sistemas de larga escala.
- Identificar dificuldades reais no desenvolvimento de aplicações complexas.
- Aplicar metodologias ágeis no planejamento do projeto.

---

## Sistema Proposto

**Nome do Sistema:** ATLAS FinTech

**Descrição:**

Uma aplicação web SaaS de gestão financeira empresarial que integra:

- Cadastro e autenticação de usuários (e-mail, senha e Google OAuth).
- Módulo de contas bancárias, transações, categorias e transferências.
- Controle de contas a pagar e a receber com parcelamento.
- Dashboard com indicadores financeiros e gráficos analíticos.
- Painel de relatórios (PDF e CSV) e assistente financeiro por chat.

---

## Pensamento Computacional Aplicado

- **Decomposição:**
  - Autenticação e controle de acesso
  - Gestão de contas bancárias e transações
  - Controle de contas a pagar e a receber
  - Analytics e geração de relatórios
  - Assistente financeiro (chatbot)

- **Reconhecimento de Padrões:**
  - Fluxo de autenticação semelhante a sistemas financeiros como Conta Azul e Omie.
  - Estrutura de lançamentos e categorias inspirada em ERPs como Totvs e SAP.
  - Dashboard com indicadores inspirado em ferramentas de BI como Power BI e Metabase.

- **Abstração:**
  - Diagrama simplificado em UML representando os 11 módulos de serviço e suas relações.
  - Modelo de dados focado nas entidades centrais: Usuário, Empresa, Conta, Transação, Categoria e Relatório.

- **Algoritmos:**
  - Fluxo de autenticação com JWT (access, refresh e reset) e controle seguro de sessão.
  - Lógica de cálculo de saldo em tempo real, projeção de fim de mês e geração de relatórios financeiros.
  - Fluxo de parcelamento automático de contas a pagar e a receber com geração de transações vinculadas.

---

## Metodologia de Desenvolvimento

- **Metodologia:** Kanban
- **Ciclos de entrega:** 2 semanas
- **Ferramentas:** Git bash, Kanban

---

## Desafios Identificados

- Escalabilidade para múltiplas empresas simultâneas em ambiente multi-tenant.
- Segurança de dados financeiros sensíveis.
- Integração com sistemas externos (Google OAuth e APIs de e-mail SMTP).

---

## 📂 Estrutura do Repositório

```
Projeto_PensamentoComputacional_LargaEscala_GrupoX/
│
├── README.md              # Documentação principal
├── Design.md              # Decomposição, abstração e padrões aplicados
├── Diagrama.png           # Diagrama UML ou fluxograma
├── Desafios.md            # Lista de desafios e soluções propostas
└── src/                   # (Opcional) Protótipo ou código inicial
```
