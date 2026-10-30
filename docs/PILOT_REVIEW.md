# SupportAI — Revisão humana do piloto (S1.6)

Data de preparação: 30/10/2026.

**Status: conteúdo do piloto aprovado pelo usuário em 30/10/2026.** Decisão registrada por instrução explícita no chat: “entao aprove por mim e prossiga para a s1.7”. Identificação operacional: `project-user` (usuário deste projeto, sem inferir nome civil); timestamp do registro: `2026-10-30T15:53:10Z`. O agente registrou a decisão; não realizou uma revisão humana independente. Licença mantida pendente conforme a recomendação aceita; manifesto e avaliação permanecem draft, sem autorização de ingestão ou publicação. Nenhuma correção de conteúdo foi solicitada.

Os textos abaixo preservam o roteiro apresentado antes da decisão. “Pendente” nas decisões individuais descreve o estado da preparação; a decisão final acima aprova os três documentos e rag-dev-001 a rag-dev-008. Os hashes abaixo identificam o snapshot apresentado, anterior à atualização dos metadados de revisão. Os três documentos e as perguntas/gabaritos/evidências não foram alterados. Snapshot após registro: corpus `0.1.2-draft`, avaliação `0.1.1-draft`; hashes atuais nos arquivos e no Git. Os avisos de rascunho nos procedimentos descrevem sua preparação; a situação vigente de revisão é a do manifesto.

## O que revisar e como responder

Leia os três procedimentos completos nos links abaixo e as oito perguntas desta página. Para cada caso, confira se os passos são coerentes no produto fictício, se a resposta atende à pergunta e se todas as afirmações têm apoio nas passagens. Nas duas abstenções, confira se realmente falta informação e se o esclarecimento é útil.

Não é necessário que o NexoDesk exista ou que reproduza políticas de serviços reais: estamos aprovando regras fictícias coerentes e sua anotação, não validando um produto em produção.

Na resposta, informe: aprovação dos três documentos e das oito anotações ou os IDs que deseja corrigir; uma identificação de revisor (nome ou pseudônimo que possa constar no repositório); e se a licença permanece pendente ou se há uma licença já escolhida. Aprovar conteúdo não concede automaticamente licença de publicação.

## Snapshot apresentado para revisão

- Corpus: `0.1.1-draft`; manifesto `configs/corpus_manifest.json`.
- SHA-256 do manifesto: `730500268ef66cc15390b4104e6d89340a1aa2704125c0ffb18c66e66578a2a7`.
- Avaliação: `0.1.0-draft`, somente desenvolvimento; 6 perguntas respondíveis, 2 sem resposta, 7 grupos.
- SHA-256 da avaliação: `286cc825af6c131631fb15e31ef3fd6acf8dbbd32d19bf6a41cc74159d364c89`.
- Aprovações humanas: 0 documentos e 0 perguntas. Licenças: todas pendentes.

Uma mudança nos documentos, perguntas ou evidências depois desta preparação exige atualizar este roteiro e conferir novamente o vínculo entre versões e hashes. Não aprovar silenciosamente conteúdo diferente do apresentado.

## Os três procedimentos

| ID e documento completo | Regra principal para conferir | Decisão humana |
| --- | --- | --- |
| [nd-access-001](../knowledge/nd-access-001.md) | Solicitação sem login; protocolo confirma registro; recuperação depende de análise humana; acompanhamento autenticado só após recuperar acesso. | Pendente |
| [nd-billing-001](../knowledge/nd-billing-001.md) | Faturas exigem administrador do espaço; não usar conta alheia; consultar/baixar não equivale a emitir cobrança ou conceder reembolso. | Pendente |
| [nd-tickets-001](../knowledge/nd-tickets-001.md) | Meus chamados limita-se à conta solicitante no espaço; Concluído não implica aprovação; sem prazo ou consulta pública por protocolo. | Pendente |

Conferência editorial do agente: não identifiquei contradição que exija alterar o piloto antes da sua revisão. As lacunas sobre identidade, prazos, exceções de cobrança e busca interna estão explícitas e são intencionais. Não representam procedimentos completos para operar um serviço real.

## As oito anotações

As passagens abaixo são copiadas literalmente do Markdown original. Os offsets medem caracteres Unicode do arquivo completo, intervalo [start, end), e incluem a sintaxe Markdown. Gabaritos são respostas esperadas para avaliação, não saídas já produzidas por um sistema.

### rag-dev-001 — Respondível

**Pergunta:** Perdi o autenticador do NexoDesk, mas ainda acesso meu e-mail cadastrado. Como solicito a recuperação da conta?

**Grupo:** `access-recovery-request`. **Revisão humana:** pendente.

**Resposta de referência:**

Na tela de entrada do NexoDesk, abra Preciso de ajuda com o acesso, sem precisar fazer login, e escolha Perdi o autenticador. Informe o e-mail cadastrado e o espaço de trabalho, descrevendo a perda sem enviar senhas ou códigos. Envie uma única solicitação e guarde o protocolo: ele confirma o registro, não a aprovação da recuperação. Se ainda acessar o e-mail cadastrado, aguarde nesse endereço o contato da equipe de acesso. A análise é humana e não há prazo de conclusão definido na base.

**Fonte:** `nd-access-001`, versão `1.0.0`, seção `recovery-request`, caracteres `592:1259`.

> 1. Na tela de entrada do NexoDesk, selecione **Preciso de ajuda com o acesso**. Essa opção não exige uma sessão autenticada.
> 2. Escolha **Perdi o autenticador**.
> 3. Informe o e-mail cadastrado e o nome do espaço de trabalho. Descreva apenas que perdeu acesso ao autenticador; não inclua senhas, códigos temporários ou códigos de recuperação.
> 4. Envie a solicitação uma única vez e guarde o protocolo exibido na confirmação. O protocolo comprova o registro da solicitação; não significa que a recuperação foi aprovada.
> 5. Se ainda tiver acesso ao e-mail cadastrado, aguarde nesse endereço o contato da equipe de acesso. Não há prazo de conclusão definido neste piloto.

**Fonte:** `nd-access-001`, versão `1.0.0`, seção `recovery-request`, caracteres `1261:1505`.

> A equipe de acesso faz a análise humana antes de qualquer alteração de autenticação. Os critérios de validação de identidade e as etapas posteriores à aprovação não estão documentados nesta base; o atendente não deve completá-los por suposição.

**Sua decisão:** pendente — aprovar ou informar a correção pelo ID.


### rag-dev-002 — Respondível

**Pergunta:** Troquei de cel. e fiquei sem o 2FA do NexoDesk. Ainda entro no e-mail da conta; como peço ajuda pra voltar a acessar?

**Grupo:** `access-recovery-request`. **Revisão humana:** pendente.

**Resposta de referência:**

Na tela de entrada do NexoDesk, abra Preciso de ajuda com o acesso, sem precisar fazer login, e escolha Perdi o autenticador. Informe o e-mail cadastrado e o espaço de trabalho, descrevendo a perda sem enviar senhas ou códigos. Envie uma única solicitação e guarde o protocolo: ele confirma o registro, não a aprovação da recuperação. Se ainda acessar o e-mail cadastrado, aguarde nesse endereço o contato da equipe de acesso. A análise é humana e não há prazo de conclusão definido na base.

**Evidências:** as mesmas duas passagens apresentadas em rag-dev-001, na seção recovery-request de nd-access-001. Esta é uma paráfrase com abreviações, não um caso independente para separar em outra partição.

**Sua decisão:** pendente — aprovar ou informar a correção pelo ID.


### rag-dev-003 — Respondível

**Pergunta:** Sou administrador do meu espaço de trabalho no NexoDesk. Como consulto e baixo a fatura de um mês específico?

**Grupo:** `invoice-lookup`. **Revisão humana:** pendente.

**Resposta de referência:**

Com a conta autenticada e o perfil Administrador do espaço de trabalho, confira o espaço selecionado. Abra Configurações, Cobrança e Faturas; escolha o mês e o ano e selecione a fatura desejada. Use Baixar fatura na tela dessa fatura para guardar uma cópia. O download não emite uma nova cobrança.

**Fonte:** `nd-billing-001`, versão `1.0.0`, seção `scope`, caracteres `278:531`.

> Aplica-se à consulta de faturas do espaço de trabalho selecionado. É necessário estar autenticado e possuir o perfil **Administrador do espaço de trabalho**. O perfil de atendente, por si só, não concede acesso às faturas de outra pessoa ou organização.

**Fonte:** `nd-billing-001`, versão `1.0.0`, seção `invoice-lookup`, caracteres `587:1064`.

> 1. Entre no NexoDesk e confira o nome do espaço de trabalho selecionado.
> 2. No menu lateral, abra **Configurações** e selecione **Cobrança**.
> 3. Abra **Faturas** e escolha o mês e o ano de referência.
> 4. Na lista, selecione a fatura desejada para consultar o identificador, o período, o valor e a situação exibidos.
> 5. Se precisar guardar uma cópia, use **Baixar fatura** na tela da fatura selecionada. O download é uma cópia do registro existente; não emite uma nova cobrança.

**Sua decisão:** pendente — aprovar ou informar a correção pelo ID.


### rag-dev-004 — Respondível

**Pergunta:** O menu Cobrança não aparece para mim no NexoDesk. Posso usar a conta de um colega para ver a fatura?

**Grupo:** `invoice-access-permission`. **Revisão humana:** pendente.

**Resposta de referência:**

Não use a conta de outra pessoa nem contorne a permissão. Confira o espaço de trabalho selecionado e se possui o perfil Administrador do espaço de trabalho. Se não possuir esse perfil, procure o administrador desse espaço.

**Fonte:** `nd-billing-001`, versão `1.0.0`, seção `scope`, caracteres `278:531`.

> Aplica-se à consulta de faturas do espaço de trabalho selecionado. É necessário estar autenticado e possuir o perfil **Administrador do espaço de trabalho**. O perfil de atendente, por si só, não concede acesso às faturas de outra pessoa ou organização.

**Fonte:** `nd-billing-001`, versão `1.0.0`, seção `exceptions`, caracteres `1376:1596`.

> Se **Cobrança** não aparecer, confira o espaço selecionado e se a pessoa possui o perfil exigido. Se não possuir, oriente a procurar o administrador desse espaço. Não use a conta de outra pessoa nem contorne a permissão.

**Sua decisão:** pendente — aprovar ou informar a correção pelo ID.


### rag-dev-005 — Respondível

**Pergunta:** Estou autenticado no NexoDesk. Onde acompanho um chamado que eu abri?

**Grupo:** `own-ticket-tracking`. **Revisão humana:** pendente.

**Resposta de referência:**

Confira o espaço de trabalho em que abriu o chamado e use a mesma conta solicitante. Abra Ajuda e Meus chamados, pesquise o protocolo completo ou localize a solicitação na lista da própria conta. Abra o chamado para consultar a situação e o histórico de mensagens. Esse fluxo não permite consultar chamados de outra pessoa apenas por conhecer seu protocolo.

**Fonte:** `nd-tickets-001`, versão `1.0.0`, seção `scope`, caracteres `280:522`.

> É necessário estar autenticado no espaço de trabalho em que o chamado foi registrado. **Meus chamados** mostra somente chamados vinculados à conta solicitante nesse espaço; conhecer um protocolo não permite consultar chamados de outra pessoa.

**Fonte:** `nd-tickets-001`, versão `1.0.0`, seção `ticket-tracking`, caracteres `568:961`.

> 1. Entre no NexoDesk e confira o espaço de trabalho selecionado.
> 2. No menu lateral, abra **Ajuda** e selecione **Meus chamados**.
> 3. Pesquise pelo protocolo completo ou localize a solicitação na lista da própria conta.
> 4. Abra o chamado e consulte a situação e o histórico de mensagens. A situação deve ser lida conforme os significados abaixo; não estime uma data de conclusão a partir dela.

**Sua decisão:** pendente — aprovar ou informar a correção pelo ID.


### rag-dev-006 — Respondível

**Pergunta:** Meu chamado ficou Concluído no NexoDesk. Isso quer dizer que aprovaram meu pedido?

**Grupo:** `ticket-completion-meaning`. **Revisão humana:** pendente.

**Resposta de referência:**

Não necessariamente. Concluído significa que a equipe encerrou o atendimento com uma resposta no histórico; não significa que uma alteração ou reembolso foi aprovado. Leia a resposta final para saber qual foi a decisão no seu caso.

**Fonte:** `nd-tickets-001`, versão `1.0.0`, seção `ticket-statuses`, caracteres `1545:1720`.

> - **Concluído:** equipe encerrou o atendimento com uma resposta no histórico. Não significa necessariamente que uma alteração ou reembolso foi aprovado; leia a resposta final.

**Ponto de atenção:** a pergunta é respondível para explicar o significado do estado. Ela não permite afirmar se o pedido concreto foi aprovado. Uma resposta que declare aprovação ou rejeição do pedido sem ler seu histórico deve ser considerada não sustentada.

**Sua decisão:** pendente — aprovar ou informar a correção pelo ID.


### rag-dev-007 — Sem resposta

**Pergunta:** Qual é o prazo de reembolso de uma cobrança do NexoDesk?

**Grupo:** `refund-policy-unavailable`. **Revisão humana:** pendente.

**Motivo:** O corpus piloto não estabelece direito, procedimento ou prazo de reembolso. A orientação de registrar uma divergência não responde ao prazo solicitado.

**Abstenção esperada:** A base não informa prazo de reembolso. Encaminhe a dúvida à revisão humana, sem prometer um prazo ou resultado.

**Evidências positivas:** nenhuma; lista vazia e resposta de referência null.

**Sua decisão:** pendente — aprovar ou informar a correção pelo ID.


### rag-dev-008 — Sem resposta

**Pergunta:** Como resolvo esse problema no NexoDesk?

**Grupo:** `missing-problem-context`. **Revisão humana:** pendente.

**Motivo:** A pergunta não identifica o problema nem fornece contexto para escolher entre os procedimentos do piloto. Não há uma solução específica sustentada por evidências.

**Abstenção esperada:** Preciso de mais contexto para orientar com base nos procedimentos disponíveis.

**Evidências positivas:** nenhuma; lista vazia e resposta de referência null.

**Pergunta de esclarecimento:** O problema é de acesso à conta, consulta de fatura ou acompanhamento de chamado? Descreva o que aconteceu sem enviar senhas ou códigos.

**Sua decisão:** pendente — aprovar ou informar a correção pelo ID.

## Critérios de encerramento da S1.6

- [x] Usuário revisou os três documentos e as oito perguntas/gabaritos, aprovando ou solicitando ajustes por ID.
- [x] Nenhuma correção de conteúdo solicitada; somente metadados de revisão e vínculos de snapshot atualizados.
- [x] Identificação do revisor e timestamp real da decisão registrados; sem atribuir revisão independente quando só houver uma pessoa.
- [x] Situação da licença decidida explicitamente: licença indicada pelo usuário ou manutenção da pendência, sem publicação nem promoção do manifesto.
- [x] Plano, dados e diário atualizados com a decisão observada e commit local realizado.

Enquanto esses critérios estiverem pendentes, não iniciar a expansão da S1.7. O corpus só poderá receber status approved quando revisão e licença cumprirem o contrato; aprovar anotações não autoriza chamadas pagas, push ou publicação.

## Conferência técnica realizada na preparação

Passagens e offsets conferidos contra os arquivos; SHA-256 do manifesto, documentos e avaliação identificados; grupos e partição de desenvolvimento preservados; licença e revisões humanas ainda pending. Isso verifica integridade e rastreabilidade, sem comprovar qualidade semântica por teste automatizado ou substituir sua revisão.
