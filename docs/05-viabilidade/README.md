# 05 — Análise de Viabilidade e Gestão de Riscos (T3/T4)

## Viabilidade Técnica e Governança SAP
* A stack proposta (**Vue.js 3**, **Node.js 20 LTS** e **PostgreSQL 16**) é madura e dominada pela equipe.
* A integração com o **SAP GUI / ERP** depende de autorização formal da TI corporativa do Grupo CGB (`RES01`). Para reduzir esse gargalo, o adaptador do backend foi projetado em duas camadas: conector REST/HTTPS para integração síncrona oficial e importação validada de relatórios estruturados exportados do SAP GUI como contingência operacional.
* Em segurança e LGPD (`RES02`), aplicam-se controles proporcionais ao risco: tráfego via TLS 1.3, hash de senhas (Argon2id/bcrypt), tokens JWT de curta duração, controle de acesso baseado em papéis (RBAC), mascaramento parcial de CPF nas tabelas gerais e retenção restrita de logs de auditoria conforme o compliance.

## Viabilidade Econômica (Com Valoração de Esforço)

| Categoria de Custo / Benefício | Estimativa / Premissa | Fundamentação e Origem dos Dados |
| :--- | :--- | :--- |
| **Custo de Licenciamento** | R$ 0,00 | Uso integral de tecnologias open-source (Vue.js, Node.js, PostgreSQL, Linux). |
| **Esforço de Desenvolvimento (CAPEX Interno)** | 160 horas totais (Valoração ref.: R$ 5.600,00) | Premissa acadêmica/interna: 2 desenvolvedores x 10h/semana x 8 semanas, a R$ 35,00/hora técnica júnior para composição de custo do projeto (sem desembolso direto em caixa). |
| **Infraestrutura Recorrente (OPEX)** | R$ 123,33 / mês | VPS Cloud (2 vCPU, 4 GB RAM, SSD) cotada em R$ 120,00/mês (Hostinger Brasil, 2026) + domínio corporativo (~R$ 40,00/ano rateado). |
| **Retorno Operacional Esperado** | Economia de ~60h/mês de retrabalho | Estimativa baseada na observação de discovery: eliminação de ~15h semanais de transcrição manual duplicada entre SAP GUI e Excel Online e redução de falhas de faturamento. |

## Viabilidade Operacional e de Prazo
* **Operacional:** o fluxo TO-BE mantém a familiaridade da equipe com o SAP GUI e substitui a planilha manual por uma tela web com validação e bloqueio de erros na origem (`RF05`), com treinamento estimado de 2 horas práticas por colaborador.
* **Prazo:** o cronograma de 8 semanas é viável desde que a homologação de acesso/layout com a TI corporativa ocorra até o fim da Semana 2, evitando gargalos na integração (Semanas 3 a 5).

## Matriz de Riscos Priorizados

| ID | Risco Identificado | Prob. | Impacto | Responsável | Mitigação e Plano de Contingência |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **R01** | Atraso ou negativa da TI corporativa na liberação de API/scripts do SAP GUI. | Alta | Crítico | Kauan / Natan | Protocolar solicitação formal na Semana 1; contingência via importação validada de arquivo estruturado exportado do SAP GUI (`FA01` de `UC03`). |
| **R02** | Divergência de schema ou layout nos dados exportados do SAP GUI. | Alta | Alto | Natan Axwell | Validação rígida de DTOs no backend Node.js rejeitando payloads malformatados antes da gravação. |
| **R03** | Instabilidade na conexão de internet local em Altamira. | Média | Alto | Natan Axwell | Armazenar rascunhos temporariamente no navegador via `IndexedDB` com reenvio automático ao restabelecer a rede. |
| **R04** | Exposição indevida de dados pessoais sensíveis (LGPD / `RES02`). | Baixa | Crítico | Kauan / Natan | Criptografia TLS 1.3, controle de acesso via JWT/RBAC e mascaramento parcial de CPF nas listagens de auditoria. |
| **R05** | Resistência operacional dos usuários na transição do Excel para o sistema web. | Média | Médio | Kauan Moreira | Workshop prático de 2h mostrando o ganho de tempo e operação em paralelo assistido na Semana 8. |
| **R06** | Sobrecarga de demandas acadêmicas/profissionais da equipe de 2 integrantes. | Média | Médio | Kauan / Natan | Sprints semanais curtas com escopo congelado no MVP (`Must Have`) e reuniões semanais de acompanhamento. |