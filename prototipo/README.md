# Protótipo Navegável — Integração de Negociações (CGB Energia)

- **Link do protótipo (Figma Make):** https://www.figma.com/proto/f6ODemunvW6a7453ePdIlK/Integracao-Negociacoes-CGB-Protótipo?node-id=1-3&p=f&t=CmGnSE5i2dBVBmKj-0&scaling=min-zoom&content-scaling=fixed&page-id=0%3A1&starting-point-node-id=1%3A3
- **Projeto:** Integração de Negociações e Cobrança — CGB Energia (Altamira - PA)
- **Autores:** Kauan Moreira do Nascimento e Natan Axwell Carvalho dos Santos
- **Orientador:** Prof. Guibson Krause (Faculdade Serra Dourada)

## Objetivo do protótipo

Simular visualmente o fluxo de registro, validação preventiva, conferência e homologação de negociações, apoiando a documentação de análise de sistemas sem depender de código de produção.

## Fluxos e telas contemplados

- **TELA-01 (Acesso e RBAC):** identificação/acesso e direcionamento conforme o perfil (`Auxiliar Administrativo` ou `Analista/Gestor`).
- **TELA-02 (Cadastro em 3 blocos e confirmação):** preenchimento de dados vinculados ao SAP, condições financeiras e contato/observações (incluindo a exceção `"Cliente sem número"`), com confirmação registrando matrícula e data/hora.
- **TELA-03 (Validação e bloqueio preventivo):** comportamento de bloqueio ao tentar avançar/salvar negociação `"Parcelada"` sem entrada e sem `"Sem número"`, com aviso superior e destaque visual nos campos pendentes.
- **TELA-04 (Auditoria e homologação de lote):** visualização para análise/gestão com filtros, CPF parcialmente mascarado, indicação de conformidade e ação de homologação de lote.

## Observações

- Protótipo de simulação e navegação desenvolvido para fins acadêmicos.
- Não representa sistema em produção nem execução de regras oficiais em ambiente real.