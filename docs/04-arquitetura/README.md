# 04 — Modelo de Domínio, Arquitetura C4 e ADR-001 (T3)

## Modelo Conceitual de Domínio

O modelo de domínio consolida as entidades centrais do MVP, seus atributos, métodos e relacionamentos:

| Entidade | Atributos e Métodos Principais | Relacionamentos e Cardinalidades |
| :--- | :--- | :--- |
| **Colaborador** | `idColaborador: int`, `nome: String`, `perfilAcesso: Enum (AUXILIAR, ANALISTA, GESTOR)`, `matricula: String` | 1 Colaborador registra `0..*` Negociações (rastreabilidade obrigatória — `RF03`). |
| **Cliente** | `contaContrato: String`, `nomeCliente: String`, `cpfCnpj: String`, `instalacao: String`, `telefone: String` | 1 Cliente possui `1..*` Negociações vinculadas à sua Conta Contrato (`RN03`). |
| **Negociacao** | `idNegociacao: int`, `numeroNota: String`, `valorNegociado: double`, `tipoPagamento: Enum`, `valorEntrada: double`, `qtdParcelas: int`, `dataCriacao: DateTime`, `statusGeracao: String`, `observacao: String`; Método: `+validarObrigatorios()` | Entidade central; pertence a 1 `Colaborador`, 1 `Cliente` e 1 `LoteAuditoria`. |
| **LoteAuditoria** | `idLote: int`, `dataFechamento: Date`, `statusHomologacao: Enum (ABERTO, HOMOLOGADO, DEVOLVIDO)`, `totalRegistros: int`; Método: `+homologarLote()` | 1 LoteAuditoria agrupa `1..*` Negociações para conferência e fechamento mensal (`UC04`). |

---

## Arquitetura C4 (Nível 1: Contexto e Nível 2: Containers)

* **Nível 1 — Diagrama de Contexto:** posiciona o **Sistema de Integração e Controle CGB** no centro, interagindo com os perfis **Auxiliar Administrativo** (lança e valida negociações) e **Analista / Gestor** (audita e homologa lotes), além de sincronizar dados de notas e contas contrato com o sistema externo legado **SAP GUI / ERP**.
* **Nível 2 — Diagrama de Containers:**
  1. **Aplicação Web (SPA — Vue.js 3 / TypeScript):** interface responsiva no navegador das estações de trabalho, com validação instantânea (`RF05`), fila de contingência offline (`IndexedDB`) e painéis de auditoria via JSON / HTTPS (TLS 1.3).
  2. **API Backend REST (Node.js 20 LTS / Express):** aplica regras de negócio (`RN01`–`RN04`), autenticação JWT, controle de acesso RBAC, mascaramento LGPD (`RES02`), trilhas de auditoria e adaptador de integração com o SAP.
  3. **Banco de Dados Relacional (PostgreSQL 16):** armazena negociações, clientes, lotes de auditoria e logs imutáveis com integridade transacional ACID via SQL (TCP 5432).

---

## ADR-001: Comparação de Alternativas Arquiteturais (Monólito Modular vs. Microsserviços)

Decisão adotada: **Monólito Modular**.

| Critério de Análise | Alternativa A: Monólito Modular (Escolhida) | Alternativa B: Microsserviços Distribuídos |
| :--- | :--- | :--- |
| **Custo de Implantação** | Baixo: VPS única (2 vCPU / 4 GB RAM) estimada em R$ 120,00/mês, sem orquestradores pagos. | Alto: exige múltiplos nós em nuvem, brokers de mensageria e cluster Kubernetes. |
| **Complexidade Técnica, Manutenção e Evolução** | Baixa/Média: base de código coesa, deploy único e transações ACID nativas no PostgreSQL; adequada a equipe enxuta (2 desenvolvedores). | Alta: exige transações distribuídas (Saga), descoberta de serviços e rastreamento distribuído; sobrecarga incompatível com a equipe local de Altamira. |
| **Segurança e LGPD** | Centralizada: autenticação JWT única, RBAC coeso e tabela centralizada de logs de auditoria. | Descentralizada: múltiplas superfícies de ataque internas exigindo mTLS e API Gateway robusto. |
| **Adequação ao Porte** | Adequada à volumetria administrativa regional sem overengineering. | Desproporcional ao volume de transações diárias da operação local. |