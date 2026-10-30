# SupportAI — S1.7, lote 1 para revisão

Registro histórico do primeiro lote. O corpus final e os sete lotes estão em [S1_7_REVIEW.md](S1_7_REVIEW.md).

Preparado em 30/10/2026. Status: conferência editorial do agente concluída; revisão humana dos três novos documentos pendente. A aprovação da S1.6 abrange somente o piloto original.

## Conteúdo do lote

| Documento completo | O que conferir | Limites preservados |
| --- | --- | --- |
| [nd-tickets-002](../knowledge/nd-tickets-002.md) — Abertura de chamado | Conta e espaço corretos; Ajuda → Novo chamado; assunto/descrição; protocolo confirma registro. | Não substitui recuperação sem login, não garante resultado ou prazo; envio incerto não autoriza reenvio repetido. |
| [nd-tickets-003](../knowledge/nd-tickets-003.md) — Responder a pedido de informação | Chamado próprio em Aguardando solicitante; ler pedido; Adicionar resposta → Enviar resposta; conferir histórico. | Não enviar segredos, não prometer mudança automática de situação nem reabertura. |
| [nd-tickets-004](../knowledge/nd-tickets-004.md) — Filtros de Meus chamados | Filtro Situação; Todas as situações; protocolo completo; combinação das condições. | Filtro não amplia permissão, Concluído não significa aprovado; lista vazia não prova exclusão. |

São escolhas fictícias reversíveis: nomes dos campos Assunto e Descrição, botão Enviar resposta, filtro Situação e aplicação automática do filtro. Nenhuma delas é uma interface implementada ou uma regra de um serviço real. Metadados e hashes estão no manifesto `0.1.3-draft`; os três novos documentos têm versão `1.0.0`, licença e revisão pendentes.

## Conferência editorial e exercício manual

- Abrir uma divergência de fatura: nd-billing-001 e nd-tickets-002 usam Ajuda → Novo chamado; o identificador da fatura é suficiente para descrever a divergência, sem dados de cartão ou promessa de reembolso.
- Perder o autenticador: nd-access-001 continua sendo a entrada sem login. Nenhum fluxo novo permite usar Meus chamados sem autenticação.
- Responder a uma solicitação de informação: localizar chamado próprio, ler histórico e verificar a mensagem enviada. O documento não inventa prazo ou estado posterior.
- Procurar chamado Concluído com filtro Em análise: limpar a restrição antes de concluir ausência. Isso complementa nd-tickets-001 sem mudar seus estados ou permissões.
- Os novos textos não estabelecem prazo de reembolso nem permitem resolver um problema sem contexto; as duas abstenções do piloto continuam semanticamente adequadas. Isso é conferência editorial, sem executar avaliação de modelo.

## Rastreabilidade e próximo lote

A avaliação `0.1.1-draft` permanece ligada ao snapshot de três documentos `0.1.2-draft`, preservado no commit `7acb503`. Não foi associada silenciosamente ao corpus expandido. Antes de avaliar o novo corpus, será preciso revisar a respondibilidade e evidências e versionar esse vínculo explicitamente. Não há teste reservado ou métricas de desempenho.

Na preparação deste lote, o corpus tinha 6 documentos (3 aprovados no piloto, 3 novos pendentes), com meta de 24. A expansão restante foi autorizada na instrução posterior “termine a s1.7”; os lotes foram preparados e conferidos editorialmente, sem atribuir aprovação humana aos novos textos. Não preencher 24 documentos repetindo o mesmo procedimento.

Para revisar, leia os três documentos completos e informe aprovação ou correções pelos IDs. Licença permanece pendente; nenhum documento está autorizado para ingestão ou publicação.
