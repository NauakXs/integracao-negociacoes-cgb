# 02 — Requisitos, Regras de Negócio, LGPD e Histórias de Usuário (T1/T2)

## Evolução e Saneamento de Requisitos
* **RNF02 -> RN04 (Reclassificação):** a regra que impede salvar registros com campos obrigatórios em branco (exceto telefone) passou de requisito não funcional para regra de negócio, por tratar de integridade de domínio.
* **RF05 (Inclusão):** alerta visual imediato na interface durante o preenchimento para dar retorno instantâneo ao lançador.
* **RES02 (Detalhamento LGPD):** explicitação dos dados pessoais tratados (Nome, CPF, Conta Contrato, Instalação, Endereço, Foto e Valores), controle de acesso por perfil (RBAC) e mascaramento.
* **HU03 / CA03 (Complementação):** inclusão de critérios de aceitação verificáveis (Dado/Quando/Então) para validação de campos obrigatórios e dependências.

## Requisitos Funcionais (RF)
* **RF01 (Must Have):** captura ou importação automatizada dos dados da negociação gerada no SAP GUI para a base central, substituindo a transcrição manual no Excel Online.
* **RF02 (Must Have):** validação obrigatória das dependências lógicas entre campos no lançamento (ex.: exigir entrada e quantidade de parcelas quando o tipo for parcelado).
* **RF03 (Must Have):** registro automático da matrícula/ID do auxiliar lançador e data/hora (timestamp) na criação e alteração de cada registro.
* **RF04 (Should Have):** painel de auditoria para o analista com filtros por operador, período e status para conferência e homologação de lotes antes do fechamento.
* **RF05 (Must Have):** bloqueio de submissão e alerta visual imediato destacando cada campo obrigatório ausente ou malformatado durante o cadastro.

## Requisitos Não Funcionais (RNF)
* **RNF01 (Should Have — Desempenho):** validação síncrona e salvamento/sincronização de transação individual em tempo de resposta igual ou inferior a 3 segundos.
* **RNF02 (Could Have — Portabilidade):** operação responsiva em navegadores corporativos modernos (Chrome e Edge) em estações desktop sem instalação local.
* **RNF03 (Must Have — Disponibilidade):** disponibilidade mínima de 99,9% nos cinco últimos dias úteis de cada mês (fechamento de metas).
* **RNF04 (Should Have — Segurança):** comunicação via TLS 1.3 (HTTPS) e logs de auditoria armazenados de forma imutável no banco com acesso restrito.
* **RNF05 (Could Have — Usabilidade):** contraste adequado (padrão WCAG AA) e navegação por teclado para reduzir fadiga visual em jornadas contínuas.

## Regras de Negócio (RN) e Restrições (RES)
* **RN01 (Must Have):** negociações parceladas exigem valor de entrada e quantidade de parcelas antes da liberação do boleto.
* **RN02 (Must Have):** somente usuários autenticados com perfil `Analista` ou `Gestor` podem alterar o status de homologação após o lançamento inicial do `Auxiliar`.
* **RN03 (Must Have):** toda negociação deve estar vinculada a número de `Nota` e `Conta Contrato` válidos e existentes na base do SAP.
* **RN04 (Must Have):** todos os campos do formulário são obrigatórios no fechamento mensal, exceto o telefone do cliente, que pode ser marcado explicitamente como `"Sem número"`.
* **RES01 (Obrigatória):** a integração com o SAP GUI deve seguir protocolos, perfis de rede e autorizações formais da TI corporativa do Grupo CGB.
* **RES02 (Obrigatória — LGPD):** dados pessoais (Nome, CPF, Conta Contrato, Instalação, Endereço, Foto e Valores) restritos a colaboradores autorizados, com mascaramento parcial de CPF em listagens gerais.

## Histórias de Usuário e Critérios de Aceitação
* **HU01:** como Auxiliar Administrativo, quero registrar a negociação do SAP GUI no sistema integrado para consolidar os dados sem redigitação no Excel.
  * *Normal:* com Nota e Conta Contrato válidas do SAP e dados conformes, o sistema salva, vincula matrícula e timestamp e exibe confirmação.
  * *Erro:* Nota ou Conta Contrato inválida/duplicada rejeita a gravação e informa inconsistência cadastral (`RF01`, `RF03`, `RN03`).
* **HU02:** como Analista Responsável, quero auditar e homologar lotes em painel consolidado para garantir conformidade financeira antes do fechamento.
  * *Normal:* filtrando por período/operador e acionando `Homologar Lote`, o sistema atualiza o status do lote e bloqueia edições.
  * *Erro:* usuário `Auxiliar` tentando acessar ou alterar status de auditoria é bloqueado por RBAC (`RF04`, `RN02`).
* **HU03:** como Auxiliar Administrativo, quero validação de obrigatoriedade e dependências em tempo real para impedir gravações incompletas.
  * *Normal:* lançamento parcelado com entrada preenchida e telefone marcado como `"Sem número"` é aceito ao salvar.
  * *Erro:* campo obrigatório vazio ou parcelamento sem entrada bloqueia o envio e destaca campos em vermelho (`RF02`, `RF05`, `RN01`, `RN04`).