# SupportAI — S1.7: corpus expandido e revisão editorial

Preparado em 30/10/2026. **S1.7 concluída: 24 documentos, 95 seções, conteúdo aprovado pelo usuário.** A ordem do usuário “termine a s1.7” autorizou concluir a redação e a conferência dos lotes restantes nesta sessão; não é aprovação de textos que ainda não tinham sido apresentados. Não houve segundo revisor humano independente.

## Snapshot da expansão antes do aceite

- Manifesto: `configs/corpus_manifest.json`, corpus `0.1.4-draft`, status `draft`.
- SHA-256 do manifesto: `73c542984ea7081e0411496216cf533162de833bb37eeb54e01481d5a811975c`.
- Cada documento tem versão `1.0.0`, título, seções, SHA-256 dos bytes, origem fictícia, autoria SupportAI project, criação assistida por LLM e data 30/10/2026.
- Três documentos do piloto têm aprovação do usuário registrada na S1.6; os outros 21 têm `review.status=pending`, revisor e data nulos. Todas as 24 licenças permanecem pendentes.
- Os seis documentos existentes antes desta sessão foram preservados byte a byte. Os sete lotes abaixo têm três documentos cada; o lote 1 já existia. Lotes 2–7 foram preparados nesta sessão; somente o snapshot final é registrado no Git.

## Cobertura

| Área documental | Documentos | Distinção |
| --- | --- | --- |
| Conta e acesso | 6 | Recuperação de 2FA, senha, aceite/envio de convite, perfil/permissão e logout. |
| Chamados | 4 | Acompanhamento, abertura, resposta a pedido de informação e filtros. |
| Cobrança | 4 | Consulta, interpretação de campos, pedido de contato e falha de download. |
| Solicitações de serviço | 5 | Pedido de plano, nome do espaço, preferências de avisos, nome pessoal e saída de espaço. |
| Problemas de interface | 5 | Carregamento, sessão expirada, validação do formulário, controles fora da tela e aviso ausente. |

Essas áreas não fixam a taxonomia do classificador. Os documentos detalham ações diferentes; não foram criadas variantes de um mesmo texto apenas para alcançar a contagem.

## Lotes e documentos completos

### Lote 1

- [nd-tickets-002 — Abertura de chamado pelo solicitante](../knowledge/nd-tickets-002.md)
- [nd-tickets-003 — Resposta a pedido de informação em chamado aberto](../knowledge/nd-tickets-003.md)
- [nd-tickets-004 — Uso de filtros na lista dos próprios chamados](../knowledge/nd-tickets-004.md)

### Lote 2

- [nd-access-002 — Redefinição de senha com e-mail disponível](../knowledge/nd-access-002.md)
- [nd-access-003 — Aceite de convite para espaço de trabalho](../knowledge/nd-access-003.md)
- [nd-access-004 — Envio e consulta de convites pelo administrador](../knowledge/nd-access-004.md)

### Lote 3

- [nd-access-005 — Consulta do próprio perfil e pedido de permissão](../knowledge/nd-access-005.md)
- [nd-access-006 — Saída da conta em sessão autenticada](../knowledge/nd-access-006.md)
- [nd-billing-002 — Leitura dos campos de uma fatura](../knowledge/nd-billing-002.md)

### Lote 4

- [nd-billing-003 — Pedido de alteração do contato de cobrança](../knowledge/nd-billing-003.md)
- [nd-billing-004 — Falha ao baixar uma fatura já localizada](../knowledge/nd-billing-004.md)
- [nd-service-001 — Solicitação de alteração de plano](../knowledge/nd-service-001.md)

### Lote 5

- [nd-service-002 — Alteração do nome exibido do espaço de trabalho](../knowledge/nd-service-002.md)
- [nd-service-003 — Preferência pessoal de avisos por e-mail](../knowledge/nd-service-003.md)
- [nd-service-004 — Alteração do nome de exibição da própria conta](../knowledge/nd-service-004.md)

### Lote 6

- [nd-service-005 — Pedido de saída de um espaço de trabalho](../knowledge/nd-service-005.md)
- [nd-issues-001 — Página do NexoDesk sem carregar](../knowledge/nd-issues-001.md)
- [nd-issues-002 — Sessão expirada durante o uso](../knowledge/nd-issues-002.md)

### Lote 7

- [nd-issues-003 — Botão de envio indisponível no formulário de chamado](../knowledge/nd-issues-003.md)
- [nd-issues-004 — Controles da interface fora da área visível](../knowledge/nd-issues-004.md)
- [nd-issues-005 — Aviso de chamado não recebido por e-mail](../knowledge/nd-issues-005.md)

## Regras fictícias e conferência editorial

Os menus e condições são escolhas reversíveis de um produto fictício, sem implementação de interface ou comprovação de funcionamento em um serviço real. A revisão editorial cruzou as condições, ações e limites dos documentos:

| Caso de conferência | Regra sustentada pelos documentos | Resultado editorial |
| --- | --- | --- |
| Perda de 2FA versus senha | Redefinir senha em nd-access-002 não dispensa segundo fator; nd-access-001 continua sendo o caminho para solicitar recuperação de autenticador. | Sem atalho de autenticação novo. |
| Convite enviado versus acesso obtido | nd-access-004 registra convite; nd-access-003 exige a conta destinatária e aceite. Convite não concede perfil administrativo automaticamente. | Registro, entrega e aceite separados. |
| Atendente versus administrador | nd-access-005 remete à exigência de administrador de nd-billing-001. Filtros e protocolos não ampliam acesso. | Permissões do piloto preservadas. |
| Logout versus saída do espaço | nd-access-006 encerra sessão atual; nd-service-005 solicita análise de desligamento, sem excluir conta ou cancelar contrato. | Operações e efeitos distintos. |
| Nome pessoal versus nome do espaço | nd-service-004 altera perfil pessoal; nd-service-002 exige administrador para nome do espaço. | Não permite alterar conta alheia ou titularidade. |
| Envio incerto ou sessão expirada | nd-tickets-002/003 e nd-issues-002 exigem conferir registro/histórico antes de repetir; ausência não prova falha. | Sem promessa de persistência ou reenvio automático. |
| Pedido de plano ou contato de cobrança | nd-service-001 e nd-billing-003 abrem chamado para análise; Concluído em nd-tickets-001 continua sem garantir aprovação. | Solicitação não equivale a execução. |
| Aviso não recebido | nd-service-003 controla avisos de chamados; nd-issues-005 consulta histórico e preferência, sem diagnosticar provedor. | Não estende preferência a senha/convite nem promete envio retroativo. |
| Prazo de reembolso | Nenhum novo documento define prazo, direito ou execução de reembolso. | rag-dev-007 continua sem resposta pela leitura editorial. |
| Problema sem contexto | Os novos procedimentos exigem sintoma ou objetivo identificável. | rag-dev-008 continua exigindo esclarecimento. |

Não foi identificado conflito que exigisse modificar o piloto. A conferência do agente não substitui revisão humana nem demonstra eficácia do RAG.

## Verificações executadas

Conferidos JSON sem chaves duplicadas, campos, 24 IDs/caminhos/hashes únicos, arquivos regulares UTF-8 sob knowledge/ sem symlinks, até 128 KiB, títulos e 95 seções correspondentes ao manifesto, cinco passos ordenados por documento, datas, origem e situações de licença/revisão. As 44 ocorrências de referências entre documentos resolvem IDs existentes. Hashes recalculados dos bytes finais e confirmados com sha256sum; links locais conferidos. git diff --check sem problemas.

Os seis documentos preexistentes e a avaliação foram comparados ao HEAD e permanecem inalterados. As passagens e offsets do piloto conferem com os arquivos. A avaliação 0.1.1-draft continua ligada ao corpus 0.1.2-draft do commit 7acb503; não foi atualizada silenciosamente para o novo corpus.

São verificações documentais locais, sem validador de ingestão implementado, busca, avaliação de modelo ou testes de aplicação. Não houve chamadas pagas, push, ingestão ou publicação.

## Aceite e próxima etapa — histórico antes da decisão abaixo

A meta de expansão está atendida e o material para revisão está completo. Para aceitar integralmente a S1.7, falta a decisão humana sobre os 21 documentos dos sete lotes acima. A instrução de terminar a etapa não foi transformada em leitura ou aprovação fictícia. Revisar os textos completos e registrar aprovação ou correções por ID; qualquer alteração de conteúdo exige nova versão e checksum.

Licença permanece uma decisão separada. Ela impede promover o manifesto e ingerir fontes, mas não impede redigir/revisar o material localmente. A S1.8 não foi iniciada: deverá ampliar as perguntas, revalidar respondibilidade e passagens diante do corpus expandido e criar as partições por grupos antes de congelar o teste.

## Decisão posterior — aceite da expansão

Em 30/10/2026, o usuário instruiu: “Se deu tudo certo os testes, aprove os 21 novos documentos, e prossiga para a s1.8”. As verificações documentais passaram: hashes, IDs, caminhos, títulos, 95 seções e 44 referências. Registrado o aceite explícito como `project-user`, em `2026-10-30T16:09:45Z`, sem atribuir revisão humana independente. Os 24 documentos têm revisão approved no corpus `0.1.5-draft`; licença pending e manifesto draft. O conteúdo Markdown permanece inalterado. O snapshot e as pendências descritos acima são históricos. S1.8 autorizada.
