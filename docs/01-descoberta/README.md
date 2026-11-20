# 01 — Descoberta, Stakeholders e Escopo (T1 Revisado)

## Contexto Operacional
Levantamento realizado presencialmente no setor administrativo e de cobrança do Grupo CGB em Altamira-PA (entre junho e julho de 2026), combinando observação da rotina de fechamento mensal e entrevistas com os envolvidos no fluxo. Hoje, o processo exige a transcrição manual de mais de 20 atributos por negociação entre o SAP GUI e o Excel Online (como Instalação, Conta Contrato, Nome do Cliente, CPF/CNPJ, Valor Negociado, Data da Negociação, Contato, Tipo de Pagamento, Nota, Plano, Reparcelamento, Entrada, Status de Geração e Observação).

## Stakeholders
* Auxiliar Administrativo (Lançador) — Kauan Moreira e Walyson Melo: alta influência / alto interesse. Precisa eliminar a redigitação manual entre SAP GUI e Excel Online e evitar esquecimento de campos obrigatórios sob pressão de prazo.
* Analista Responsável — Robert Paulo: média influência / alto interesse. Precisa auditar lançamentos com agilidade, filtrar pendências por operador e bloquear alterações não autorizadas após conferência.
* Gerente da Operação — Nilo Breno: média influência / alto interesse. Precisa acompanhar indicadores consolidados de produtividade e volume de acordos sem depender de planilhas desatualizadas.
* Agente de Campo / Equipe Comercial Externa: alta influência no início / alto interesse. Precisa receber rapidamente o boleto gerado após enviar o termo assinado e fotos/documentação para concluir o atendimento.
* Clientes Finais / Consumidores CGB: baixa influência direta / alto interesse. Precisam receber o boleto de negociação no prazo combinado e ter seus dados pessoais protegidos.

## Delimitação de Escopo

### Dentro do Escopo
* Recepção e conferência documental de negociações de campo.
* Consulta e validação de Conta Contrato e Nota vinculadas ao SAP GUI.
* Cadastro web integrado com validação síncrona de campos obrigatórios e dependências de pagamento (à vista ou parcelado com entrada).
* Painel de auditoria, rastreabilidade de operador/data-hora e homologação de lotes pelo analista.

### Fora do Escopo
* Alteração do código-fonte ou do banco de dados interno do ERP corporativo SAP GUI.
* Desenvolvimento de aplicativo móvel nativo para agentes de campo (mantém-se o canal atual de envio documental à central).
* Módulos contábeis ou financeiros corporativos de diretoria.