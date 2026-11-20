# Sistema de Integração e Controle de Negociações Comerciais — Grupo CGB (Altamira-PA)

> **Projeto Integrado de Análise de Sistemas (Trabalho 4)**  
> **Instituição:** Faculdade Serra Dourada — Curso de Análise e Desenvolvimento de Sistemas (ADS)  
> **Orientador:** Prof. Guibson Krause  
> **Equipe:** Kauan Moreira do Nascimento & Natan Axwell Carvalho dos Santos  
> **Ano:** 2026

---

## 1. Visão Geral e Contexto Operacional

Na unidade operacional do **Grupo CGB** em **Altamira-PA**, o registro de negociações comerciais de débitos exige que auxiliares administrativos lancem os dados manualmente no ERP corporativo **SAP GUI** e, em seguida, transcrevam essas mesmas informações (mais de 20 atributos por acordo) para uma planilha compartilhada no **Excel Online**. Essa duplicidade gera retrabalho, atraso no envio de boletos aos clientes e falhas de preenchimento nos picos de fechamento mensal.

Este repositório consolida os artefatos de **engenharia de requisitos, modelagem de processos (BPMN), casos de uso (UML), arquitetura de software (C4 e ADR), análise de viabilidade, planejamento de MVP e prototipação interativa** de um sistema web complementar projetado para eliminar a redigitação manual sem alterar o código-fonte nativo do SAP GUI.

---

## 2. Arquitetura e Stack Tecnológica

A solução adota o padrão de **Monólito Modular** (ADR-001), equilibrando baixo custo operacional, transações ACID nativas e facilidade de manutenção para o porte regional da operação:

* **Frontend (SPA):** Vue.js 3 + TypeScript (validação síncrona em tempo real, fila de contingência offline via IndexedDB e painéis de auditoria).
* **Backend (API REST):** Node.js 20 LTS + Express (regras de negócio RN01–RN04, autenticação JWT, controle de acesso RBAC, mascaramento LGPD e adaptador de integração SAP em duas camadas).
* **Banco de Dados:** PostgreSQL 16 (persistência relacional e logs de auditoria imutáveis).
* **Segurança e Conformidade (LGPD):** Tráfego TLS 1.3 (HTTPS), hash de senhas (Argon2id/bcrypt), segregação de perfis (`Auxiliar`, `Analista`, `Gestor`) e mascaramento parcial de CPF nas listagens gerais.

---

## 3. Estrutura do Repositório

```text
integracao-negociacoes-cgb/
├── README.md                           # Visão geral, contexto, arquitetura e guia de navegação
├── docs/
│   ├── 01-descoberta/                  # Roteiro de elicitação, stakeholders e delimitação de escopo (T1 revisado)
│   ├── 02-requisitos/                  # Requisitos (RF/RNF), regras de negócio (RN), restrições LGPD e HUs (T1/T2)
│   ├── 03-processos-uml/               # Diagramas BPMN (AS-IS / TO-BE), Casos de Uso UML e especificações (T2)
│   ├── 04-arquitetura/                 # Modelo de Domínio, Diagramas C4 (Níveis 1 e 2) e ADR-001 (T3)
│   ├── 05-viabilidade/                 # Análise de viabilidade (técnica, econômica, operacional, prazo) e riscos (T3/T4)
│   └── 06-planejamento-validacao/      # Backlog MoSCoW (MVP), cronograma de 8 semanas, validação e matriz mestre (T4)
└── prototipo/                          # Link público do Figma e especificações/exportações das telas TELA-01 a TELA-04