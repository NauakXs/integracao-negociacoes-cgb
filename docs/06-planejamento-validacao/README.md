# 06 — Backlog MoSCoW, Cronograma, Validação e Matriz de Rastreabilidade (T4)

## Backlog Priorizado (Método MoSCoW)

| ID | Item do Backlog | Prioridade / Escopo | Req. / HU | Critério de Aceite | Responsável |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **B01** | Autenticação JWT e controle de perfis RBAC (`Auxiliar`, `Analista`, `Gestor`). | Must Have (MVP) | `RN02`, `RES02` | Bloqueio de rotas não autorizadas (`CA02`). | Natan Axwell |
| **B02** | Formulário de cadastro de negociação com validação síncrona de campos obrigatórios e alertas visuais. | Must Have (MVP) | `RF02`, `RF05`, `RN01`, `RN04`, `HU03` | Bloqueio de campos vazios e exceção `"Sem número"` (`CA03`). | Kauan Moreira |
| **B03** | Adaptador de integração SAP (API/importação estruturada) com unicidade de Nota e Conta Contrato. | Must Have (MVP) | `RF01`, `RN03`, `RES01`, `HU01` | Sincronização em até 3s sem duplicidade (`CA01`, `RNF01`). | Natan Axwell |
| **B04** | Registro automático de matrícula do lançador, timestamp e trilha de auditoria imutável. | Must Have (MVP) | `RF03`, `RNF04` | Logs gravados em 100% das transações (`CA01`). | Natan Axwell |
| **B05** | Painel de auditoria do analista com filtros e homologação de lotes. | Should Have (MVP+) | `RF04`, `RN02`, `HU02` | Homologação bloqueia edição pelo lançador (`CA02`). | Kauan Moreira |
| **B06** | Persistência offline temporária (`IndexedDB`) para contingência de queda de rede. | Must Have (MVP) | `RNF03` | Reenvio automático após reconexão (`R03`). | Natan Axwell |
| **B07** | Módulo de consolidação de KPIs gerenciais e exportação CSV/PDF. | Could Have (Release 2) | `RF04` (`UC05`) | Totais por operador e período conferidos. | Kauan Moreira |
| **B08** | Ajustes finos de acessibilidade (contraste WCAG AA e atalhos de teclado). | Could Have (Release 2) | `RNF02`, `RNF05` | Navegação completa via teclado validada. | Kauan Moreira |

## Planejamento de Execução (8 Semanas)

| Etapa / Prazo | Atividades e Entregas (Marcos) | Esforço | Dependências Críticas | Responsável |
| :--- | :--- | :--- | :--- | :--- |
| **Etapa 1 (Semanas 1–2)** | Setup do repositório, modelagem física PostgreSQL, autenticação JWT/RBAC (`B01`) e protocolo junto à TI SAP. | 40 horas | Aprovação formal do layout/acesso pela TI corporativa (`R01`). | Natan Axwell |
| **Etapa 2 (Semanas 3–5)** | Construção da API Node.js, regras `RN01`–`RN04`, adaptador SAP (`B03`) e trilha de auditoria (`B04`). Marco: Core API concluído. | 60 horas | Conclusão do banco físico e definição de DTOs do SAP (Etapa 1). | Natan / Kauan |
| **Etapa 3 (Semanas 6–7)** | Desenvolvimento da SPA Vue.js, formulário com alertas visuais (`B02`) e painel de auditoria (`B05`). Marco: MVP funcional. | 40 horas | Endpoints REST documentados e estabilizados (Etapa 2). | Kauan Moreira |
| **Etapa 4 (Semana 8)** | Testes integrados, validação com usuários, treinamento de 2h e implantação assistida em VPS. Marco: Go-Live MVP. | 20 horas | Homologação do analista (Robert Paulo) e ambiente VPS ativo. | Kauan / Natan |

## Roteiro e Registro de Validação (Revisão por Pares)

* **VAL01:** registro de negociação parcelada completa na `TELA-02` em menos de 45 segundos sem planilhas externas — concluído em 32s; adicionado campo calculado de conferência do valor total parcelado.
* **VAL02:** bloqueio de salvamento incompleto na `TELA-03` (`RF05` / `RN01`) — bloqueio imediato e refinamento do texto de alerta para diferenciar entrada obrigatória de campo vazio.
* **VAL03:** exceção de cliente sem telefone (`RN04`) na `TELA-02` — desabilitou obrigatoriedade do telefone ao marcar `"Sem número"` e gravou a string padronizada `"SEM NUMERO"`.
* **VAL04:** filtragem de pendências, mascaramento de CPF e homologação de lote na `TELA-04` (`RES02`) — CPF exibido parcialmente mascarado na grade principal e detalhado apenas no modal auditado do analista.

*Nota de transparência:* a avaliação inicial dos fluxos e telas foi conduzida por revisão por pares entre Kauan Moreira e Natan Axwell antes da apresentação formal ao analista Robert Paulo e ao gerente Nilo Breno.

## Matriz de Rastreabilidade Integrada

| Req. | Regra / Rest. | História | Atividade BPMN TO-BE | Caso de Uso | Protótipo | Backlog / Validação |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **RF01** | `RN03`, `RES01` | `HU01` | Integração Automática de Dados (Raia 2) | `UC01`, `UC03` | `TELA-02` | `B03` / `VAL01` |
| **RF02** | `RN01`, `RN04` | `HU03` | Integração Automática de Dados (Raia 2) | `UC02` | `TELA-02`, `TELA-03` | `B02` / `VAL02`, `VAL03` |
| **RF03** | `RNF04`, `RES02` | `HU01` | Integração Automática de Dados (Raia 2) | `UC03` | `TELA-01`, `TELA-02` | `B01`, `B04` / `VAL01` |
| **RF04** | `RN02`, `RES02` | `HU02` | Validar Lote de Negociações e Relatórios (Raia 3) | `UC04`, `UC05` | `TELA-04` | `B05`, `B07` / `VAL04` |
| **RF05** | `RN01`, `RN04` | `HU03` | Integração Automática de Dados (Raia 2) | `UC02` | `TELA-03` | `B02` / `VAL02` |