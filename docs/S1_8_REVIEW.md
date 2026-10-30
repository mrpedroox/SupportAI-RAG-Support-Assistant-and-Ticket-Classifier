# SupportAI — S1.8: perguntas, evidências e partições para revisão

Preparado em 30/10/2026. **Preparação e conferência editorial concluídas; 42 novas anotações aguardam revisão humana.** As oito do piloto continuam aprovadas e em desenvolvimento. Licença pendente. Não há métricas de modelo, ingestão ou teste congelado.

## Decisões e rastreabilidade

- Avaliação `0.2.0-draft`, vinculada explicitamente ao corpus `0.1.5-draft` pelo SHA-256 do manifesto.
- `evaluation/rag_split_manifest.json` registra caminhos, checksums, IDs, grupos e contagens. Arquivos: `rag_development.json` (35) e `rag_test.json` (15).
- O piloto `rag_dev_pilot.json` fica preservado, ligado ao snapshot original. Suas oito anotações foram copiadas sem mudar IDs, textos, gabaritos, passagens ou aprovações. Conferência editorial diante dos 24 documentos: as seis respostas continuam sustentadas; não foi criado prazo de reembolso e a pergunta vaga continua sem contexto. Motivos que mencionam o piloto mantêm sua redação de origem.
- Atribuição manual de grupos antes das partições, sem sorteio ou seed. Grupos representam famílias de intenção, incluindo paráfrases e exceções do mesmo procedimento. Nenhum grupo aparece nas duas partições. Compartilhar documento entre partições é permitido para consultas sobre base fixa; não alegamos generalização para documentos novos.
- Desenvolvimento: 28 respondíveis, 7 sem resposta. Teste: 11 respondíveis, 4 sem resposta. Total: 39 respondíveis e 11 sem resposta. Desenvolvimento tem 22 grupos; teste tem 11 grupos.
- Novos casos em lotes de 9, 9, 9, 8 e 7. As evidências são passagens de seções originais com offsets Unicode `[start, end)` e checksum do documento. Algumas incluem contexto além do mínimo; a revisão deve conferir a pertinência e a cobertura de cada afirmação. A correspondência com chunks fica para a Sprint 2.
- Teste proposto e ainda não congelado: o agente o leu para anotação/conferência, sem usar resultados de modelo ou ajustar prompts, top-k, chunking e limiares. Após revisão humana, congelar versão/checksum. Antes disso, não apresentar o conjunto como teste final intocado. Não usar suas perguntas para desenvolvimento do sistema; se influenciar decisões, registrar exposição e revisar a reserva.
- Origem fictícia e assistência de LLM declaradas. Aceite dos documentos não aprova estas perguntas novas. Não há segundo anotador humano independente. Nenhuma chamada paga ou publicação.

## Como revisar

Leia pergunta, gabarito e passagens em conjunto. Para respondíveis, confira se cada afirmação está sustentada e se falta algum passo necessário. Para não respondíveis, confira se a resposta realmente está ausente em todo o corpus ou se falta contexto. Registre correções ou aceite pelos IDs após leitura. Não completar regras ausentes com conhecimento externo. A suíte de prompt injection continua separada.

## Lote 1 — 9 anotações novas

### rag-dev-009 — development — grupo `password-reset`

**Pergunta:** Esqueci minha senha, mas leio o e-mail da conta. Como redefino a senha do NexoDesk?

**Gabarito proposto:** Na tela de entrada, use Esqueci minha senha, informe o e-mail cadastrado e solicite uma redefinição. Confira entrada e mensagens indesejadas. Se receber a mensagem solicitada e o destino for confiável, preencha Nova senha e Confirmar nova senha e salve. Após confirmação, entre com a nova senha e o segundo fator quando exigido.

**Evidência:** [nd-access-002, v1.0.0, seção scope](../knowledge/nd-access-002.md), caracteres [77, 487).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à pessoa que esqueceu a senha, conhece o e-mail cadastrado e ainda consegue ler esse e-mail. Redefinir a senha não remove a exigência do autenticador. Para perda do segundo fator, consulte `nd-access-001`.

**Evidência:** [nd-access-002, v1.0.0, seção password-reset](../knowledge/nd-access-002.md), caracteres [531, 1274).

> 1. Na tela de entrada, selecione **Esqueci minha senha**.
> 2. Informe o e-mail cadastrado e selecione **Solicitar redefinição** uma única vez. Não envie a senha antiga ao suporte.
> 3. Confira a caixa de entrada e a pasta de mensagens indesejadas. A mensagem na tela confirma a solicitação, mas não comprova a entrega do e-mail nem a existência de uma conta.
> 4. Se receber a mensagem de redefinição solicitada, abra o link e preencha **Nova senha** e **Confirmar nova senha**, conforme as exigências exibidas na própria tela. Se o destino parecer suspeito, não informe credenciais e encaminhe à revisão humana.
> 5. Selecione **Salvar senha**. Somente após a confirmação de alteração tente entrar com a nova senha e o segundo fator, quando exigido.

Revisão humana: **pending**.

### rag-dev-010 — development — grupo `password-reset`

**Pergunta:** Não lembro a senha; ainda tenho meu e-mail. Qual o caminho pra trocar a senha e entrar?

**Gabarito proposto:** Na tela de entrada, use Esqueci minha senha, informe o e-mail cadastrado e solicite uma redefinição. Confira entrada e mensagens indesejadas. Se receber a mensagem solicitada e o destino for confiável, preencha Nova senha e Confirmar nova senha e salve. Após confirmação, entre com a nova senha e o segundo fator quando exigido.

**Evidência:** [nd-access-002, v1.0.0, seção scope](../knowledge/nd-access-002.md), caracteres [77, 487).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à pessoa que esqueceu a senha, conhece o e-mail cadastrado e ainda consegue ler esse e-mail. Redefinir a senha não remove a exigência do autenticador. Para perda do segundo fator, consulte `nd-access-001`.

**Evidência:** [nd-access-002, v1.0.0, seção password-reset](../knowledge/nd-access-002.md), caracteres [531, 1274).

> 1. Na tela de entrada, selecione **Esqueci minha senha**.
> 2. Informe o e-mail cadastrado e selecione **Solicitar redefinição** uma única vez. Não envie a senha antiga ao suporte.
> 3. Confira a caixa de entrada e a pasta de mensagens indesejadas. A mensagem na tela confirma a solicitação, mas não comprova a entrega do e-mail nem a existência de uma conta.
> 4. Se receber a mensagem de redefinição solicitada, abra o link e preencha **Nova senha** e **Confirmar nova senha**, conforme as exigências exibidas na própria tela. Se o destino parecer suspeito, não informe credenciais e encaminhe à revisão humana.
> 5. Selecione **Salvar senha**. Somente após a confirmação de alteração tente entrar com a nova senha e o segundo fator, quando exigido.

Revisão humana: **pending**.

### rag-dev-011 — development — grupo `password-reset`

**Pergunta:** Esqueci a senha e perdi também o acesso ao e-mail cadastrado. A base garante recuperar minha conta?

**Gabarito proposto:** Não há garantia de recuperação sem o e-mail. Encaminhe essa limitação à revisão humana e não use o endereço de outra pessoa.

**Evidência:** [nd-access-002, v1.0.0, seção exceptions](../knowledge/nd-access-002.md), caracteres [1319, 1583).

> Se o link for recusado, volte à tela de entrada e solicite uma nova mensagem. Não repita pedidos sucessivos se a entrega falhar. Se não receber a mensagem ou também tiver perdido o e-mail, encaminhe a limitação à revisão humana; não use o endereço de outra pessoa.

**Evidência:** [nd-access-002, v1.0.0, seção limits](../knowledge/nd-access-002.md), caracteres [1622, 1822).

> Não há prazo de entrega, duração do link ou critérios de senha definidos nesta base. Não invente esses valores. Não se garante recuperação sem e-mail, encerramento de outras sessões ou remoção de 2FA.

Revisão humana: **pending**.

### rag-dev-012 — development — grupo `invitation-acceptance`

**Pergunta:** Recebi um convite esperado para um espaço. Como aceito sem entrar com a conta errada?

**Gabarito proposto:** Confira com quem combinou o convite, o espaço e o destino do link. Abra o convite esperado, confira destinatário e espaço e entre com a conta do e-mail convidado. Aceite e aguarde confirmação; confira o seletor de espaços. Se estiver em outra conta, saia dela antes. O aceite não atribui automaticamente perfil administrativo.

**Evidência:** [nd-access-003, v1.0.0, seção invitation-acceptance](../knowledge/nd-access-003.md), caracteres [522, 1242).

> 1. Confira quem combinou o convite e o nome do espaço. Se não reconhecer a solicitação ou o destino do link, não informe credenciais; peça confirmação ao administrador por um contato já conhecido.
> 2. Abra o convite esperado. Confira o e-mail destinatário e o espaço apresentados na tela.
> 3. Entre com a conta correspondente ao e-mail convidado. Se a tela exigir criar uma conta que ainda não existe, siga apenas os campos e requisitos apresentados; este procedimento não define um cadastro alternativo.
> 4. Confira o espaço e selecione **Aceitar convite**. Aguarde a confirmação.
> 5. Confira se o espaço ficou disponível no seletor. Aceitar convite não atribui automaticamente o perfil Administrador do espaço de trabalho.

**Evidência:** [nd-access-003, v1.0.0, seção exceptions](../knowledge/nd-access-003.md), caracteres [1287, 1535).

> Se estiver usando outra conta, saia dela conforme `nd-access-006` e entre com a conta convidada. Se o convite aparecer inválido ou não permitir o aceite, peça ao administrador para verificar seu registro conforme `nd-access-004`; não altere o link.

Revisão humana: **pending**.

### rag-dev-013 — development — grupo `invitation-acceptance`

**Pergunta:** Me convidaram pro NexoDesk. Posso aceitar com qualquer uma das minhas contas?

**Gabarito proposto:** Use a conta correspondente ao e-mail destinatário do convite. Não encaminhe o convite para outra conta. Se estiver em outra conta, saia dela e entre com a conta convidada.

**Evidência:** [nd-access-003, v1.0.0, seção scope](../knowledge/nd-access-003.md), caracteres [76, 463).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à pessoa que recebeu um convite esperado para um espaço do NexoDesk. O convite destina-se ao endereço informado pelo administrador; não deve ser encaminhado para uso por outra conta.

**Evidência:** [nd-access-003, v1.0.0, seção invitation-acceptance](../knowledge/nd-access-003.md), caracteres [522, 1242).

> 1. Confira quem combinou o convite e o nome do espaço. Se não reconhecer a solicitação ou o destino do link, não informe credenciais; peça confirmação ao administrador por um contato já conhecido.
> 2. Abra o convite esperado. Confira o e-mail destinatário e o espaço apresentados na tela.
> 3. Entre com a conta correspondente ao e-mail convidado. Se a tela exigir criar uma conta que ainda não existe, siga apenas os campos e requisitos apresentados; este procedimento não define um cadastro alternativo.
> 4. Confira o espaço e selecione **Aceitar convite**. Aguarde a confirmação.
> 5. Confira se o espaço ficou disponível no seletor. Aceitar convite não atribui automaticamente o perfil Administrador do espaço de trabalho.

**Evidência:** [nd-access-003, v1.0.0, seção exceptions](../knowledge/nd-access-003.md), caracteres [1287, 1535).

> Se estiver usando outra conta, saia dela conforme `nd-access-006` e entre com a conta convidada. Se o convite aparecer inválido ou não permitir o aceite, peça ao administrador para verificar seu registro conforme `nd-access-004`; não altere o link.

Revisão humana: **pending**.

### rag-dev-014 — development — grupo `workspace-invitation`

**Pergunta:** Sou administrador. Como convido um membro comum para meu espaço?

**Gabarito proposto:** Confira o espaço e abra Configurações, Pessoas, Convites. Confira se o endereço já consta antes de criar outro convite. Se não houver, selecione Convidar pessoa, confira o e-mail autorizado e envie uma única vez. Confira a entrada na lista e oriente o destinatário a aceitar. O registro não comprova entrega ou aceite nem concede perfil administrativo.

**Evidência:** [nd-access-004, v1.0.0, seção scope](../knowledge/nd-access-004.md), caracteres [82, 499).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Exige conta autenticada com perfil Administrador do espaço de trabalho selecionado. Convidar uma pessoa não autoriza compartilhar credenciais. Este fluxo permite convite de membro comum, sem conceder perfil administrativo.

**Evidência:** [nd-access-004, v1.0.0, seção workspace-invitation](../knowledge/nd-access-004.md), caracteres [548, 1237).

> 1. Confira o espaço de trabalho e abra **Configurações**, **Pessoas**, **Convites**.
> 2. Antes de criar outro convite, confira se o e-mail esperado já aparece na lista de convites. Use apenas dados necessários da pessoa que autorizou o convite.
> 3. Se não houver convite para esse endereço, selecione **Convidar pessoa** e preencha **E-mail**. Confira cuidadosamente o destinatário; não use um endereço de teste pertencente a terceiros.
> 4. Selecione **Enviar convite** uma única vez. Aguarde a confirmação e confira a entrada na lista.
> 5. Oriente a pessoa destinatária a seguir `nd-access-003`. A presença na lista confirma o registro do convite; não comprova entrega da mensagem nem aceite.

Revisão humana: **pending**.

### rag-dev-015 — development — grupo `workspace-invitation`

**Pergunta:** Já existe um convite pro mesmo e-mail, mas a pessoa não consegue aceitar. Envio outro?

**Gabarito proposto:** Não crie duplicatas. Confira a situação exibida e peça revisão humana se a pessoa não puder aceitar. A base não define reenvio, cancelamento ou edição do destinatário.

**Evidência:** [nd-access-004, v1.0.0, seção exceptions](../knowledge/nd-access-004.md), caracteres [1282, 1567).

> Se já houver um convite para o endereço, não crie duplicatas. Confira a situação exibida e peça revisão humana se a pessoa não puder aceitar. Esta base não define reenvio, cancelamento ou edição do destinatário. Se o menu não aparecer, confira o perfil e o espaço sem usar outra conta.

Revisão humana: **pending**.

### rag-dev-016 — development — grupo `invoice-access-permission`

**Pergunta:** Como descubro meu perfil no espaço e peço a permissão que falta?

**Gabarito proposto:** Confira o espaço, abra o menu da conta e Meu perfil e leia Perfil no espaço. Compare com o requisito do procedimento. Se faltar permissão, procure o administrador e explique a finalidade sem compartilhar credenciais. Aguarde a decisão e confira o perfil antes de tentar novamente; o pedido não muda o acesso sozinho.

**Evidência:** [nd-access-005, v1.0.0, seção permission-request](../knowledge/nd-access-005.md), caracteres [559, 1235).

> 1. Confira o espaço selecionado. Abra o menu da conta e selecione **Meu perfil**.
> 2. Leia o campo **Perfil no espaço**. Ele informa o perfil da própria conta no espaço atual, sem listar permissões de outras pessoas.
> 3. Compare o requisito do procedimento pretendido com o perfil exibido. Para faturas, `nd-billing-001` exige Administrador do espaço de trabalho.
> 4. Se faltar a permissão exigida, procure o administrador do espaço e descreva o menu necessário e a finalidade, sem compartilhar senha ou códigos.
> 5. Aguarde a decisão do responsável. Um pedido registrado ou uma descrição de cargo não alteram o perfil; confirme novamente o campo antes de tentar o fluxo restrito.

Revisão humana: **pending**.

### rag-dev-017 — development — grupo `ticket-creation`

**Pergunta:** Estou autenticado. Como abro um chamado comum no NexoDesk?

**Gabarito proposto:** Confira o espaço, abra Ajuda e Novo chamado. Preencha Assunto e Descrição com o que tentou, o resultado e o esperado, removendo segredos e dados pessoais. Revise e envie uma única vez; aguarde confirmação. Guarde o protocolo e acompanhe com a mesma conta e espaço. Registro não equivale a aprovação ou prazo.

**Evidência:** [nd-tickets-002, v1.0.0, seção scope](../knowledge/nd-tickets-002.md), caracteres [71, 557).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à abertura de um chamado comum por uma pessoa autenticada no espaço de trabalho correto. Não substitui a solicitação de recuperação de acesso de `nd-access-001`, que começa na tela de entrada sem login. O assistente pode explicar os passos, mas não envia a solicitação pela pessoa.

**Evidência:** [nd-tickets-002, v1.0.0, seção ticket-creation](../knowledge/nd-tickets-002.md), caracteres [605, 1682).

> 1. Entre no NexoDesk e confira o nome do espaço de trabalho em que precisa de suporte. Se não souber qual selecionar, peça esclarecimento antes de continuar.
> 2. No menu lateral, abra **Ajuda** e selecione **Novo chamado**.
> 3. Preencha **Assunto** com um resumo e **Descrição** com o que tentou fazer, o que aconteceu e o que esperava. Inclua a mensagem de erro, se houver, após remover dados pessoais e segredos. Não envie senhas, códigos de autenticação, dados de cartão ou dados de outras pessoas.
> 4. Confira o espaço de trabalho e o texto e selecione **Enviar** uma única vez. Aguarde a confirmação antes de tentar novamente.
> 5. Se a confirmação exibir um protocolo, guarde-o. Ele comprova o registro do chamado; não representa aprovação do pedido nem prazo de atendimento. Acompanhe a situação pelo procedimento `nd-tickets-001`, com a mesma conta solicitante e no mesmo espaço.
> 
> O registro de uma divergência de fatura segue esse caminho. Nesse caso, informe o identificador da fatura e a divergência, conforme `nd-billing-001`; não inclua comprovantes com dados pessoais.

Revisão humana: **pending**.

## Lote 2 — 9 anotações novas

### rag-dev-018 — development — grupo `ticket-creation`

**Pergunta:** Preciso mandar uma solicitação de suporte pela minha conta. Onde preencho e confirmo o chamado?

**Gabarito proposto:** Confira o espaço, abra Ajuda e Novo chamado. Preencha Assunto e Descrição com o que tentou, o resultado e o esperado, removendo segredos e dados pessoais. Revise e envie uma única vez; aguarde confirmação. Guarde o protocolo e acompanhe com a mesma conta e espaço. Registro não equivale a aprovação ou prazo.

**Evidência:** [nd-tickets-002, v1.0.0, seção scope](../knowledge/nd-tickets-002.md), caracteres [71, 557).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à abertura de um chamado comum por uma pessoa autenticada no espaço de trabalho correto. Não substitui a solicitação de recuperação de acesso de `nd-access-001`, que começa na tela de entrada sem login. O assistente pode explicar os passos, mas não envia a solicitação pela pessoa.

**Evidência:** [nd-tickets-002, v1.0.0, seção ticket-creation](../knowledge/nd-tickets-002.md), caracteres [605, 1682).

> 1. Entre no NexoDesk e confira o nome do espaço de trabalho em que precisa de suporte. Se não souber qual selecionar, peça esclarecimento antes de continuar.
> 2. No menu lateral, abra **Ajuda** e selecione **Novo chamado**.
> 3. Preencha **Assunto** com um resumo e **Descrição** com o que tentou fazer, o que aconteceu e o que esperava. Inclua a mensagem de erro, se houver, após remover dados pessoais e segredos. Não envie senhas, códigos de autenticação, dados de cartão ou dados de outras pessoas.
> 4. Confira o espaço de trabalho e o texto e selecione **Enviar** uma única vez. Aguarde a confirmação antes de tentar novamente.
> 5. Se a confirmação exibir um protocolo, guarde-o. Ele comprova o registro do chamado; não representa aprovação do pedido nem prazo de atendimento. Acompanhe a situação pelo procedimento `nd-tickets-001`, com a mesma conta solicitante e no mesmo espaço.
> 
> O registro de uma divergência de fatura segue esse caminho. Nesse caso, informe o identificador da fatura e a divergência, conforme `nd-billing-001`; não inclua comprovantes com dados pessoais.

Revisão humana: **pending**.

### rag-dev-019 — development — grupo `ticket-creation`

**Pergunta:** Cliquei em Enviar no novo chamado, mas não vi protocolo. Devo mandar tudo de novo?

**Gabarito proposto:** Não afirme que foi registrado nem reenvie repetidamente. Confira Meus chamados com a mesma conta e espaço. Se localizar, acompanhe; se não localizar ou não conseguir conferir, encaminhe à revisão humana antes de novo envio. Ausência na lista não prova falha do registro.

**Evidência:** [nd-tickets-002, v1.0.0, seção exceptions](../knowledge/nd-tickets-002.md), caracteres [1730, 2343).

> Se **Novo chamado** não aparecer, confira se está autenticado e no espaço esperado. Se continuar ausente, registre a limitação para revisão humana; não use a conta de outra pessoa. Este procedimento não documenta alteração de permissões.
> 
> Se ocorrer erro ou não houver protocolo, não afirme que o envio foi registrado nem reenvie repetidamente. Confira **Meus chamados** com a mesma conta e no mesmo espaço. Se localizar o chamado, acompanhe-o; se não localizar ou não conseguir conferir, encaminhe a limitação à revisão humana antes de novo envio. A ausência na lista não prova, por si só, que o registro falhou.

Revisão humana: **pending**.

### rag-dev-020 — development — grupo `ticket-information-response`

**Pergunta:** Meu chamado está Aguardando solicitante e pediram mais informações. Como respondo?

**Gabarito proposto:** Abra Ajuda e Meus chamados na conta e espaço corretos, localize seu chamado e leia o pedido no histórico. Use Adicionar resposta, informe somente o necessário sem segredos, revise e envie uma vez. Confira a mensagem no histórico. Registro não comprova aprovação nem permite estimar prazo.

**Evidência:** [nd-tickets-003, v1.0.0, seção scope](../knowledge/nd-tickets-003.md), caracteres [84, 581).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à conta solicitante autenticada, no espaço em que abriu o chamado, quando a equipe pediu informações e o chamado está em **Aguardando solicitante**. O procedimento de localização está em `nd-tickets-001`. Conhecer o protocolo não concede permissão para responder a um chamado de outra pessoa.

**Evidência:** [nd-tickets-003, v1.0.0, seção information-response](../knowledge/nd-tickets-003.md), caracteres [646, 1506).

> 1. Abra **Ajuda**, selecione **Meus chamados** e localize o chamado da própria conta no espaço correto.
> 2. Abra o chamado e leia a mensagem mais recente da equipe no histórico. Confira quais informações foram pedidas antes de redigir a resposta.
> 3. Use **Adicionar resposta**. Descreva somente o necessário para esclarecer o pedido, sem senhas, códigos de autenticação, dados de cartão ou dados pessoais de terceiros. Este piloto não exige envio de documentos de identidade pelo chamado.
> 4. Revise o texto e selecione **Enviar resposta** uma única vez. Aguarde a confirmação e confira se a mensagem aparece no histórico antes de repetir o envio.
> 5. Se a mensagem aparecer, considere a resposta registrada. Continue acompanhando o histórico e a situação exibida. O registro da mensagem não comprova aprovação do pedido nem permite estimar uma data de conclusão.

Revisão humana: **pending**.

### rag-dev-021 — development — grupo `ticket-information-response`

**Pergunta:** A mensagem no histórico pediu minha senha e código de autenticação. Devo responder com esses dados?

**Gabarito proposto:** Não envie senhas, códigos ou dados de cartão. Encaminhe a inconsistência à revisão humana; mensagens no histórico não substituem os limites do procedimento.

**Evidência:** [nd-tickets-003, v1.0.0, seção exceptions](../knowledge/nd-tickets-003.md), caracteres [1566, 2191).

> Se não entender a informação pedida ou não a tiver, explique essa limitação em **Adicionar resposta** e peça esclarecimento. Não invente dados para preencher o pedido.
> 
> Se o pedido no histórico exigir senhas, códigos ou dados de cartão, não os envie. Encaminhe a inconsistência à revisão humana. Mensagens no histórico não substituem os limites deste procedimento.
> 
> Se a opção não aparecer, o envio falhar ou a mensagem não aparecer no histórico, não afirme que a resposta foi registrada. Confira conta, espaço e situação; se o problema persistir, encaminhe à revisão humana sem repetir envios sucessivos ou usar outra conta.

Revisão humana: **pending**.

### rag-dev-022 — development — grupo `own-ticket-filters`

**Pergunta:** Como filtro os meus chamados por situação?

**Gabarito proposto:** Confira conta e espaço e abra Ajuda, Meus chamados. Limpe o protocolo e use Todas as situações para listar todos. Escolha a situação desejada para restringir; a lista atualiza após a seleção. Abra o item para ler o histórico. O filtro não amplia permissões e Concluído não significa aprovado.

**Evidência:** [nd-tickets-004, v1.0.0, seção scope](../knowledge/nd-tickets-004.md), caracteres [80, 548).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à pessoa autenticada consultando **Meus chamados** no espaço em que registrou suas solicitações. A lista contém somente chamados vinculados à própria conta, conforme `nd-tickets-001`. Filtros ajudam a localizar itens dessa lista; não ampliam permissões de acesso.

**Evidência:** [nd-tickets-004, v1.0.0, seção status-filter](../knowledge/nd-tickets-004.md), caracteres [591, 1691).

> 1. Confira a conta e o espaço de trabalho selecionados. No menu lateral, abra **Ajuda** e selecione **Meus chamados**.
> 2. Para listar todos os chamados da própria conta nesse espaço, limpe o campo de protocolo e selecione **Todas as situações** no filtro **Situação**.
> 3. Para restringir a lista, selecione uma situação: **Recebido**, **Em análise**, **Aguardando solicitante** ou **Concluído**. A lista é atualizada após a seleção; não há botão adicional de aplicação neste fluxo.
> 4. Abra um item da lista para consultar o histórico. Interprete o estado conforme `nd-tickets-001`; um filtro por **Concluído** não identifica pedidos aprovados.
> 5. Para procurar um protocolo específico, volte a **Todas as situações** e informe o protocolo completo no campo de pesquisa. Ao terminar, limpe o campo se quiser voltar à lista completa da própria conta.
> 
> Quando um protocolo e uma situação específica estiverem preenchidos ao mesmo tempo, a lista mostrará somente chamados que atendam às duas condições. Se não houver resultado, remova a restrição de situação antes de concluir que o chamado está ausente.

Revisão humana: **pending**.

### rag-dev-023 — development — grupo `own-ticket-filters`

**Pergunta:** Busquei um protocolo com o filtro Em análise e não apareceu. Isso prova que foi excluído?

**Gabarito proposto:** Não. Protocolo e situação são condições combinadas. Selecione Todas as situações, confira o protocolo completo, a conta e o espaço. Se continuar ausente, siga a revisão humana; lista vazia não comprova exclusão.

**Evidência:** [nd-tickets-004, v1.0.0, seção status-filter](../knowledge/nd-tickets-004.md), caracteres [591, 1691).

> 1. Confira a conta e o espaço de trabalho selecionados. No menu lateral, abra **Ajuda** e selecione **Meus chamados**.
> 2. Para listar todos os chamados da própria conta nesse espaço, limpe o campo de protocolo e selecione **Todas as situações** no filtro **Situação**.
> 3. Para restringir a lista, selecione uma situação: **Recebido**, **Em análise**, **Aguardando solicitante** ou **Concluído**. A lista é atualizada após a seleção; não há botão adicional de aplicação neste fluxo.
> 4. Abra um item da lista para consultar o histórico. Interprete o estado conforme `nd-tickets-001`; um filtro por **Concluído** não identifica pedidos aprovados.
> 5. Para procurar um protocolo específico, volte a **Todas as situações** e informe o protocolo completo no campo de pesquisa. Ao terminar, limpe o campo se quiser voltar à lista completa da própria conta.
> 
> Quando um protocolo e uma situação específica estiverem preenchidos ao mesmo tempo, a lista mostrará somente chamados que atendam às duas condições. Se não houver resultado, remova a restrição de situação antes de concluir que o chamado está ausente.

**Evidência:** [nd-tickets-004, v1.0.0, seção exceptions](../knowledge/nd-tickets-004.md), caracteres [1744, 2122).

> Se não houver resultados, limpe a pesquisa, selecione **Todas as situações** e confira conta e espaço. Se o chamado continuar ausente, siga a orientação de revisão humana de `nd-tickets-001`; não conclua que foi excluído.
> 
> Se o filtro ou o campo de pesquisa não aparecer, registre a limitação para revisão humana. Não invente outro menu ou prometa acesso por um endereço direto.

Revisão humana: **pending**.

### rag-dev-024 — development — grupo `invoice-field-interpretation`

**Pergunta:** O campo Período da fatura é o vencimento? O protocolo do chamado serve como identificador da fatura?

**Gabarito proposto:** Período identifica a referência do registro e não deve ser tratado como prazo de pagamento. Use o Identificador da própria fatura ao relatar divergência, sem substituí-lo pelo protocolo do chamado.

**Evidência:** [nd-billing-002, v1.0.0, seção invoice-fields](../knowledge/nd-billing-002.md), caracteres [503, 1187).

> 1. Localize a fatura pelo mês e ano em **Configurações**, **Cobrança**, **Faturas**, conforme `nd-billing-001`.
> 2. Confira o **Identificador** da fatura. Use esse identificador ao relatar uma divergência; não o substitua pelo protocolo de um chamado.
> 3. Leia o **Período** exibido para identificar a referência do registro. Não o trate como prazo de pagamento.
> 4. Leia **Valor** e **Situação** exatamente como apresentados. O download é uma cópia do registro; não comprova pagamento ou ausência de débitos.
> 5. Se houver uma divergência, abra um chamado com identificador e descrição conforme `nd-billing-001` e `nd-tickets-002`, sem dados de cartão ou comprovantes com dados pessoais.

Revisão humana: **pending**.

### rag-dev-025 — development — grupo `plan-change-request`

**Pergunta:** Sou administrador e quero mudar de plano. Posso efetivar a mudança pelo assistente?

**Gabarito proposto:** O assistente não aprova a proposta. Confira espaço e perfil e abra Ajuda, Novo chamado com assunto Solicitação de alteração de plano. Descreva objetivo e funcionalidades necessárias sem inventar um plano. Envie uma vez, guarde protocolo e acompanhe. É um pedido para análise humana; não comprova contratação, cancelamento ou mudança efetivada.

**Evidência:** [nd-service-001, v1.0.0, seção scope](../knowledge/nd-service-001.md), caracteres [68, 444).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se ao Administrador do espaço de trabalho autenticado que quer solicitar análise de mudança de plano. O piloto não contém catálogo de planos, preços ou condições contratuais.

**Evidência:** [nd-service-001, v1.0.0, seção plan-change-request](../knowledge/nd-service-001.md), caracteres [502, 1120).

> 1. Confira o espaço de trabalho e o perfil da própria conta.
> 2. Abra **Ajuda**, **Novo chamado**, conforme `nd-tickets-002`.
> 3. No assunto, indique **Solicitação de alteração de plano**. Na descrição, informe o objetivo da mudança e as funcionalidades de que precisa. Não invente o nome de um plano se não o souber.
> 4. Envie uma única solicitação e guarde o protocolo confirmado. Solicitação registrada não significa contratação, cancelamento ou mudança efetivada.
> 5. Acompanhe a resposta em **Meus chamados**. Condições e eventual autorização da mudança dependem de análise humana; o assistente não aprova a proposta.

Revisão humana: **pending**.

### rag-dev-026 — development — grupo `workspace-name-change`

**Pergunta:** Sou administrador. Como altero somente o nome exibido do espaço?

**Gabarito proposto:** Confira o espaço, abra Configurações, Espaço de trabalho e o campo Nome de exibição. Informe um nome sem dados sensíveis, respeitando a validação. Salve uma vez, aguarde confirmação e reabra para conferir. Isso não altera identificador interno, endereço ou titularidade.

**Evidência:** [nd-service-002, v1.0.0, seção scope](../knowledge/nd-service-002.md), caracteres [82, 464).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Exige Administrador do espaço de trabalho autenticado. Altera somente o nome de exibição do espaço selecionado; não altera o identificador interno, o endereço de acesso ou a titularidade.

**Evidência:** [nd-service-002, v1.0.0, seção workspace-name-change](../knowledge/nd-service-002.md), caracteres [518, 1093).

> 1. Confira cuidadosamente o espaço que deseja alterar e abra **Configurações**, **Espaço de trabalho**.
> 2. Localize o campo **Nome de exibição** e confira o nome atual.
> 3. Informe um nome que identifique o espaço, sem dados pessoais sensíveis ou segredos. Respeite os limites apresentados no formulário.
> 4. Selecione **Salvar** uma única vez e aguarde confirmação. Não considere o novo nome persistido apenas porque aparece no campo de edição.
> 5. Reabra a página de configuração e confira o nome exibido. Se a alteração não aparecer, registre a limitação para revisão humana.

Revisão humana: **pending**.

## Lote 3 — 9 anotações novas

### rag-dev-027 — development — grupo `ticket-email-preference`

**Pergunta:** Quero desativar e-mails sobre mensagens dos meus chamados. Onde configuro?

**Gabarito proposto:** Confira conta e espaço, abra Preferências no menu da conta e localize Avisos de chamados por e-mail. Escolha Desativados, salve e reabra para conferir. Continue consultando Meus chamados. A opção é pessoal, por espaço, e não controla senha, convites ou avisos administrativos.

**Evidência:** [nd-service-003, v1.0.0, seção scope](../knowledge/nd-service-003.md), caracteres [75, 532).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à pessoa autenticada no espaço desejado. A preferência controla avisos por e-mail sobre mensagens dos próprios chamados nesse espaço, sem alterar o acesso ao histórico. Não controla mensagens de recuperação de senha, convites ou avisos administrativos.

**Evidência:** [nd-service-003, v1.0.0, seção email-notification-preference](../knowledge/nd-service-003.md), caracteres [600, 1161).

> 1. Confira a conta e o espaço selecionados. Abra o menu da conta e selecione **Preferências**.
> 2. Localize **Avisos de chamados por e-mail** e confira a opção atual.
> 3. Escolha **Ativados** para solicitar esses avisos ou **Desativados** para deixar de solicitá-los. A escolha é pessoal e não muda a preferência de colegas.
> 4. Selecione **Salvar** e aguarde a confirmação. Reabra Preferências para conferir se a opção ficou registrada.
> 5. Continue consultando **Meus chamados** conforme `nd-tickets-001`; a entrega de e-mail não substitui a leitura do histórico.

Revisão humana: **pending**.

### rag-dev-028 — development — grupo `page-load-troubleshooting`

**Pergunta:** A página do NexoDesk não termina de carregar. O que posso conferir com segurança?

**Gabarito proposto:** Identifique a página e eventual texto não enviado; se necessário e permitido, preserve anotação mínima sem segredos. Confira o endereço conhecido e faça uma tentativa de recarregar. Se pedir login, siga o procedimento de sessão expirada. Se carregar, confira conta, espaço e histórico antes de reenviar. Se persistir, relate tela e erro sanitizado à revisão humana; use Ajuda se acessível. Não há canal alternativo documentado.

**Evidência:** [nd-issues-001, v1.0.0, seção page-load-troubleshooting](../knowledge/nd-issues-001.md), caracteres [519, 1341).

> 1. Identifique qual página tentou abrir e se há texto não enviado na tela. Antes de recarregar, preserve apenas uma anotação local mínima sem segredos, se necessário e permitido; não copie conteúdo privado para serviços externos.
> 2. Confira se o endereço é o acesso conhecido do NexoDesk. Não siga links desconhecidos nem forneça credenciais a páginas suspeitas.
> 3. Faça uma única tentativa de recarregar a página. Se aparecer a tela de entrada, siga `nd-issues-002`.
> 4. Se carregar, confira a conta, o espaço e o histórico antes de repetir qualquer envio que estava em andamento.
> 5. Se continuar falhando, registre qual tela falhou e a mensagem de erro sanitizada para revisão humana. Se conseguir usar Ajuda, abra um chamado conforme `nd-tickets-002`; caso contrário, informe que a base não oferece um canal alternativo.

Revisão humana: **pending**.

### rag-dev-029 — development — grupo `expired-session`

**Pergunta:** Minha sessão expirou enquanto enviava um chamado. Como evito duplicar o envio?

**Gabarito proposto:** Interrompa envios na tela antiga e anote somente a ação, sem credenciais ou conteúdo sensível. Entre novamente pela tela conhecida com a própria conta e confira espaço e conta. Consulte Meus chamados e o histórico. Se o registro estiver lá, não reenvie. Se não conseguir determinar o resultado, peça revisão humana antes de repetir.

**Evidência:** [nd-issues-002, v1.0.0, seção expired-session](../knowledge/nd-issues-002.md), caracteres [494, 1246).

> 1. Interrompa novos envios na tela antiga. Anote apenas a ação que estava tentando realizar, sem guardar credenciais ou copiar conteúdo sensível.
> 2. Volte à tela de entrada conhecida e autentique com sua própria conta. Se perdeu o autenticador, siga `nd-access-001`; não contorne o segundo fator.
> 3. Confira a conta e o espaço de trabalho após entrar.
> 4. Se estava enviando um chamado ou resposta, consulte Meus chamados e o histórico conforme `nd-tickets-001`. Se o registro aparecer, não reenvie a mesma solicitação.
> 5. Se não conseguir determinar se o envio foi registrado, encaminhe a limitação à revisão humana antes de repetir. Para alterações de preferências ou perfil, reabra a configuração e confira o valor persistido antes de nova tentativa.

Revisão humana: **pending**.

### rag-dev-030 — development — grupo `ticket-form-validation`

**Pergunta:** No formulário Novo chamado, o botão Enviar está desabilitado. O que verifico?

**Gabarito proposto:** Confira conta, espaço e formulário. Assunto e Descrição devem conter texto útil, não apenas espaços. Leia validações e ajuste os campos indicados. Quando habilitar, revise e envie uma vez, aguardando protocolo. Se continuar indisponível sem orientação suficiente, encaminhe erro sanitizado à revisão humana; não force controles pelo navegador.

**Evidência:** [nd-issues-003, v1.0.0, seção scope](../knowledge/nd-issues-003.md), caracteres [87, 516).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se ao formulário Novo chamado de `nd-tickets-002`, com conta autenticada. Assunto e Descrição são obrigatórios; espaços em branco sozinhos não preenchem esses campos. Não se aplica ao envio de respostas ou recuperação de conta.

**Evidência:** [nd-issues-003, v1.0.0, seção ticket-form-validation](../knowledge/nd-issues-003.md), caracteres [576, 1273).

> 1. Confira que está em **Ajuda**, **Novo chamado**, na conta e no espaço corretos.
> 2. Verifique se **Assunto** e **Descrição** têm texto útil. Preencha o resumo e os detalhes necessários sem senhas, códigos ou dados pessoais de terceiros.
> 3. Leia mensagens de validação próximas aos campos e ajuste apenas os valores indicados, respeitando os limites exibidos.
> 4. Se **Enviar** ficar disponível, confira o conteúdo e envie uma única vez, aguardando protocolo conforme `nd-tickets-002`.
> 5. Se continuar indisponível sem mensagem que permita corrigir o formulário, registre a limitação e o texto do erro sanitizado para revisão humana. Não altere controles internos do navegador para forçar o envio.

Revisão humana: **pending**.

### rag-dev-031 — development — grupo `password-reset`

**Pergunta:** Quantos minutos dura o link para redefinir a senha?

**Motivo de abstenção:** O corpus não define duração do link de redefinição.

**Resposta esperada:** A base não fornece informação suficiente. O corpus não define duração do link de redefinição.

Revisão humana: **pending**.

### rag-dev-032 — development — grupo `invitation-acceptance`

**Pergunta:** Em quantos dias expira um convite para o espaço?

**Motivo de abstenção:** O corpus não define prazo de validade de convites.

**Resposta esperada:** A base não fornece informação suficiente. O corpus não define prazo de validade de convites.

Revisão humana: **pending**.

### rag-dev-033 — development — grupo `ticket-cancellation-unavailable`

**Pergunta:** Qual botão cancela um chamado já aberto?

**Motivo de abstenção:** Não há procedimento de cancelamento de chamado na base.

**Resposta esperada:** A base não fornece informação suficiente. Não há procedimento de cancelamento de chamado na base.

Revisão humana: **pending**.

### rag-dev-034 — development — grupo `live-financial-data-unavailable`

**Pergunta:** Quanto estou devendo na minha fatura de outubro agora?

**Motivo de abstenção:** Os documentos são estáticos e o assistente não consulta o registro financeiro em tempo real.

**Resposta esperada:** A base não fornece informação suficiente. Os documentos são estáticos e o assistente não consulta o registro financeiro em tempo real.

Revisão humana: **pending**.

### rag-dev-035 — development — grupo `missing-problem-context`

**Pergunta:** Não consigo fazer aquilo no sistema. Como conserto?

**Motivo de abstenção:** Faltam a ação pretendida e o sintoma para selecionar um procedimento.

**Resposta esperada:** A base não fornece informação suficiente. Faltam a ação pretendida e o sintoma para selecionar um procedimento.

**Esclarecimento:** O que tentou fazer e qual mensagem ou comportamento apareceu?

Revisão humana: **pending**.

## Lote 4 — 8 anotações novas

### rag-test-001 — test — grupo `session-signout`

**Pergunta:** Como saio da sessão atual para entrar em outra conta minha?

**Gabarito proposto:** Confira se alterações e mensagens receberam confirmação. No menu da conta, selecione Sair e aguarde a tela de entrada. Só então entre com outra conta própria e confira o espaço. Não use conta de colega para obter permissões.

**Evidência:** [nd-access-006, v1.0.0, seção session-signout](../knowledge/nd-access-006.md), caracteres [495, 1126).

> 1. Antes de sair, confira se mensagens ou alterações necessárias já receberam confirmação de registro. Um texto ainda no formulário pode não estar salvo.
> 2. Abra o menu da conta e selecione **Sair**.
> 3. Aguarde o retorno à tela de entrada. Se ainda aparecer conteúdo autenticado, não considere a saída confirmada; encaminhe a limitação à revisão humana.
> 4. Para usar outra conta própria, entre pela tela de entrada com as credenciais dessa conta e confira o espaço selecionado.
> 5. Em equipamento compartilhado, não deixe credenciais ou códigos visíveis. Não entre com conta de colega para obter permissões que sua conta não possui.

Revisão humana: **pending**.

### rag-test-002 — test — grupo `session-signout`

**Pergunta:** Fechei a aba. Isso confirma que saí da conta em um computador compartilhado?

**Gabarito proposto:** Fechar a aba não comprova logout. O fluxo documentado usa Sair no menu da conta e exige aguardar a tela de entrada. Se ainda aparecer conteúdo autenticado, não considere a saída confirmada; encaminhe à revisão humana.

**Evidência:** [nd-access-006, v1.0.0, seção session-signout](../knowledge/nd-access-006.md), caracteres [495, 1126).

> 1. Antes de sair, confira se mensagens ou alterações necessárias já receberam confirmação de registro. Um texto ainda no formulário pode não estar salvo.
> 2. Abra o menu da conta e selecione **Sair**.
> 3. Aguarde o retorno à tela de entrada. Se ainda aparecer conteúdo autenticado, não considere a saída confirmada; encaminhe a limitação à revisão humana.
> 4. Para usar outra conta própria, entre pela tela de entrada com as credenciais dessa conta e confira o espaço selecionado.
> 5. Em equipamento compartilhado, não deixe credenciais ou códigos visíveis. Não entre com conta de colega para obter permissões que sua conta não possui.

**Evidência:** [nd-access-006, v1.0.0, seção limits](../knowledge/nd-access-006.md), caracteres [1451, 1635).

> Não define encerramento remoto de sessões, revogação de dispositivos ou tempo de expiração. Fechar a aba não comprova logout. Não orienta apagar dados do navegador indiscriminadamente.

Revisão humana: **pending**.

### rag-test-003 — test — grupo `billing-contact-request`

**Pergunta:** Administro o espaço e preciso solicitar mudança do contato de cobrança. Como registro?

**Gabarito proposto:** Confira espaço e perfil e abra Ajuda, Novo chamado. Informe no assunto a alteração de contato e descreva espaço e motivo, sem cartão, documentos pessoais ou e-mail de terceiros nesta demonstração. Envie uma vez, guarde protocolo e acompanhe. A coleta posterior depende de orientação humana e o registro não comprova alteração.

**Evidência:** [nd-billing-003, v1.0.0, seção scope](../knowledge/nd-billing-003.md), caracteres [77, 483).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se ao Administrador do espaço de trabalho autenticado que precisa solicitar mudança do contato usado para comunicações de cobrança. Não altera titularidade, dados fiscais, acesso às faturas ou permissões.

**Evidência:** [nd-billing-003, v1.0.0, seção billing-contact-request](../knowledge/nd-billing-003.md), caracteres [544, 1184).

> 1. Confira o espaço e seu perfil conforme `nd-access-005`.
> 2. Abra **Ajuda**, **Novo chamado**, conforme `nd-tickets-002`.
> 3. No assunto, indique que solicita alteração do contato de cobrança. Na descrição, informe o espaço e o motivo; não inclua dados de cartão, documentos pessoais ou o e-mail de terceiros nesta demonstração.
> 4. Envie uma única vez e guarde o protocolo confirmado. A equipe deverá orientar a coleta dos dados necessários em revisão humana; este piloto não documenta esse canal posterior.
> 5. Acompanhe o chamado com a própria conta. Não considere o contato alterado por ter enviado o pedido; aguarde a resposta da equipe.

Revisão humana: **pending**.

### rag-test-004 — test — grupo `billing-contact-request`

**Pergunta:** Abrir o pedido de contato financeiro já troca o contato e dá acesso às faturas?

**Gabarito proposto:** Não considere o contato alterado pelo pedido; aguarde a resposta da equipe. O procedimento não altera titularidade, dados fiscais, acesso às faturas ou permissões, nem promete privilégios administrativos.

**Evidência:** [nd-billing-003, v1.0.0, seção scope](../knowledge/nd-billing-003.md), caracteres [77, 483).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se ao Administrador do espaço de trabalho autenticado que precisa solicitar mudança do contato usado para comunicações de cobrança. Não altera titularidade, dados fiscais, acesso às faturas ou permissões.

**Evidência:** [nd-billing-003, v1.0.0, seção billing-contact-request](../knowledge/nd-billing-003.md), caracteres [544, 1184).

> 1. Confira o espaço e seu perfil conforme `nd-access-005`.
> 2. Abra **Ajuda**, **Novo chamado**, conforme `nd-tickets-002`.
> 3. No assunto, indique que solicita alteração do contato de cobrança. Na descrição, informe o espaço e o motivo; não inclua dados de cartão, documentos pessoais ou o e-mail de terceiros nesta demonstração.
> 4. Envie uma única vez e guarde o protocolo confirmado. A equipe deverá orientar a coleta dos dados necessários em revisão humana; este piloto não documenta esse canal posterior.
> 5. Acompanhe o chamado com a própria conta. Não considere o contato alterado por ter enviado o pedido; aguarde a resposta da equipe.

**Evidência:** [nd-billing-003, v1.0.0, seção limits](../knowledge/nd-billing-003.md), caracteres [1492, 1664).

> Não define prazo, cobrança pelo serviço ou validação do novo endereço. Não promete atualização retroativa de faturas nem que o contato receberá privilégios administrativos.

Revisão humana: **pending**.

### rag-test-005 — test — grupo `invoice-download-failure`

**Pergunta:** Sou administrador e abri a fatura, mas Baixar fatura não gera arquivo. Como procedo?

**Gabarito proposto:** Confira espaço, mês, ano e fatura e anote o identificador. Use Baixar fatura e confira downloads do navegador antes de repetir. Observe bloqueios ou erros sem desativar proteções ou instalar extensões. Se a sessão expirou, autentique pelo procedimento correspondente e localize a mesma fatura. Persistindo, abra chamado com identificador e erro sanitizado, sem conteúdo financeiro completo.

**Evidência:** [nd-billing-004, v1.0.0, seção scope](../knowledge/nd-billing-004.md), caracteres [75, 451).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Exige Administrador do espaço de trabalho autenticado e uma fatura existente já aberta conforme `nd-billing-001`. A falha de download não prova erro de cobrança nem altera a fatura.

**Evidência:** [nd-billing-004, v1.0.0, seção invoice-download-failure](../knowledge/nd-billing-004.md), caracteres [511, 1209).

> 1. Confira o espaço, o mês e o ano e abra a fatura desejada. Anote somente seu identificador para eventual relato.
> 2. Use **Baixar fatura** na tela da fatura. Confira a área de downloads do navegador antes de repetir a ação.
> 3. Se nenhum arquivo aparecer, observe se o navegador exibiu uma mensagem de bloqueio ou falha. Não desative proteções nem instale extensões por orientação deste procedimento.
> 4. Se a sessão tiver expirado, siga `nd-issues-002` e tente localizar novamente a mesma fatura após autenticar.
> 5. Se a falha persistir, registre um chamado com o identificador e a mensagem de erro sanitizada, conforme `nd-tickets-002`. Não envie o conteúdo financeiro completo ou dados de cartão.

Revisão humana: **pending**.

### rag-test-006 — test — grupo `invoice-download-failure`

**Pergunta:** Não encontro nenhuma fatura na lista. É o mesmo procedimento de erro no download?

**Gabarito proposto:** Não trate fatura ausente como falha de download. Confira mês, ano e espaço. Se continuar sem resultado, abra solicitação em Ajuda, Novo chamado, informando período e ausência. A lista vazia não comprova inexistência de cobrança ou cancelamento.

**Evidência:** [nd-billing-004, v1.0.0, seção exceptions](../knowledge/nd-billing-004.md), caracteres [1254, 1487).

> Se a fatura não estiver na lista, siga a exceção de ausência de `nd-billing-001`; não classifique isso como falha de download. Se baixar um arquivo inesperado, não o trate como fatura válida e encaminhe a ocorrência à revisão humana.

**Evidência:** [nd-billing-001, v1.0.0, seção exceptions](../knowledge/nd-billing-001.md), caracteres [1376, 2178).

> Se **Cobrança** não aparecer, confira o espaço selecionado e se a pessoa possui o perfil exigido. Se não possuir, oriente a procurar o administrador desse espaço. Não use a conta de outra pessoa nem contorne a permissão.
> 
> Se a lista estiver vazia, confira mês, ano e espaço de trabalho. Se continuar sem resultado, registre uma solicitação pelo menu **Ajuda**, opção **Novo chamado**, informando o período e descrevendo a ausência. Uma lista vazia não comprova que não há cobrança ou que houve cancelamento.
> 
> Em caso de valor ou situação contestados, registre um chamado pelo mesmo caminho com o identificador da fatura e uma descrição da divergência. Não envie dados de cartão, credenciais ou comprovantes com dados pessoais neste piloto. O acompanhamento do chamado está descrito em `nd-tickets-001`.

Revisão humana: **pending**.

### rag-test-007 — test — grupo `account-display-name`

**Pergunta:** Quero mudar meu nome pessoal exibido, mantendo o nome do espaço. Qual é o caminho?

**Gabarito proposto:** No menu da conta, abra Meu perfil na própria conta. Edite Nome de exibição, respeite a validação e não altere Perfil no espaço. Salve perfil uma vez, aguarde confirmação e reabra para conferir. Isso não altera e-mail de entrada, senha, 2FA ou privilégios.

**Evidência:** [nd-service-004, v1.0.0, seção scope](../knowledge/nd-service-004.md), caracteres [81, 434).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à pessoa autenticada que deseja mudar o nome exibido da própria conta. O nome da conta é distinto do nome do espaço de trabalho de `nd-service-002`.

**Evidência:** [nd-service-004, v1.0.0, seção account-display-name](../knowledge/nd-service-004.md), caracteres [487, 1005).

> 1. Abra o menu da conta e selecione **Meu perfil**. Confira que está na própria conta.
> 2. Localize **Nome de exibição**. Não altere o campo Perfil no espaço; ele não é editável por este fluxo.
> 3. Preencha o nome desejado respeitando a validação apresentada. Não inclua dados de terceiros ou informações sensíveis desnecessárias.
> 4. Selecione **Salvar perfil** uma única vez e aguarde confirmação.
> 5. Reabra Meu perfil e confira o nome registrado. Se não corresponder ao enviado, encaminhe a limitação à revisão humana.

**Evidência:** [nd-service-004, v1.0.0, seção limits](../knowledge/nd-service-004.md), caracteres [1302, 1457).

> Não altera e-mail de entrada, senha, 2FA ou privilégios. Não define se mensagens antigas passam a mostrar o novo nome nem permite renomear outros usuários.

Revisão humana: **pending**.

### rag-test-008 — test — grupo `workspace-leave-request`

**Pergunta:** Quero deixar um espaço do NexoDesk. Como peço a saída e sei se já perdi o acesso?

**Gabarito proposto:** Confira conta e espaço; se houver dúvida, esclareça antes. Abra Ajuda, Novo chamado e solicite saída, identificando espaço e intenção sem dados de outros participantes. Guarde protocolo e acompanhe enquanto houver acesso. O pedido depende de análise humana e não comprova remoção. Se perder acesso antes de acompanhar, relate a limitação; não há consulta pública prometida.

**Evidência:** [nd-service-005, v1.0.0, seção scope](../knowledge/nd-service-005.md), caracteres [75, 456).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à pessoa autenticada que quer deixar um espaço específico. É diferente de Sair da sessão em `nd-access-006`. Não há remoção automática de participação documentada neste piloto.

**Evidência:** [nd-service-005, v1.0.0, seção workspace-leave-request](../knowledge/nd-service-005.md), caracteres [522, 1213).

> 1. Confira a própria conta e o nome do espaço do qual deseja sair.
> 2. Se não souber qual espaço é o correto, peça esclarecimento antes de registrar o pedido.
> 3. Abra **Ajuda**, **Novo chamado** e informe no assunto que solicita saída do espaço. Na descrição, identifique o espaço e explique a intenção, sem listar dados de outros participantes.
> 4. Envie uma única vez e guarde o protocolo confirmado. A equipe fará análise humana; não considere o acesso removido pelo registro do pedido.
> 5. Acompanhe a resposta com a própria conta enquanto houver acesso. Se perder acesso antes de conseguir acompanhar, encaminhe essa limitação à revisão humana sem prometer consulta pública pelo protocolo.

Revisão humana: **pending**.

## Lote 5 — 7 anotações novas

### rag-test-009 — test — grupo `interface-visibility`

**Pergunta:** Um controle ficou cortado na tela, mas preciso manter meu zoom por acessibilidade. Como agir?

**Gabarito proposto:** Mantenha a configuração necessária à acessibilidade e encaminhe a limitação à revisão humana. Identifique tela e controle; não conclua perda de permissão apenas pelo sintoma. Não reduza a legibilidade para continuar.

**Evidência:** [nd-issues-004, v1.0.0, seção interface-visibility](../knowledge/nd-issues-004.md), caracteres [542, 1266).

> 1. Identifique qual controle procura e confira conta, espaço e perfil exigidos pelo procedimento correspondente.
> 2. Confira se há uma janela ou diálogo cobrindo a área desejada. Feche somente diálogos que puder descartar sem perder conteúdo necessário.
> 3. Se for confortável e adequado às suas necessidades, amplie a janela ou use a rolagem normal da página. Não reduza a legibilidade para continuar.
> 4. Se alterou o zoom e puder fazê-lo sem prejudicar o uso, teste a opção de zoom padrão do próprio navegador. A base não define atalhos por navegador.
> 5. Se o controle continuar inacessível, descreva qual tela e controle estão afetados para revisão humana. Não conclua que a conta perdeu permissão por esse sintoma isolado.

**Evidência:** [nd-issues-004, v1.0.0, seção exceptions](../knowledge/nd-issues-004.md), caracteres [1311, 1544).

> Se não puder modificar a apresentação por necessidade de acessibilidade, mantenha sua configuração e encaminhe a limitação. Se o menu estiver ausente mesmo com a interface legível, siga `nd-access-005`; não use conta de outra pessoa.

Revisão humana: **pending**.

### rag-test-010 — test — grupo `missing-ticket-notification`

**Pergunta:** Não chegou o e-mail de uma mensagem no meu chamado. Como confiro se houve resposta?

**Gabarito proposto:** Consulte o histórico em Meus chamados com sua conta e espaço. Confira a mensagem esperada e a preferência Avisos de chamados por e-mail. Se desejar avisos e estiverem desativados, ative e salve sem esperar envio retroativo. Confira entrada e mensagens indesejadas. Se a preferência estiver ativa e a dúvida persistir, encaminhe protocolo e descrição mínima à revisão humana.

**Evidência:** [nd-issues-005, v1.0.0, seção scope](../knowledge/nd-issues-005.md), caracteres [75, 460).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à ausência de um aviso por e-mail sobre mensagem em chamado próprio. Não se aplica a redefinição de senha ou convite. Não receber e-mail não prova que a equipe deixou de responder.

**Evidência:** [nd-issues-005, v1.0.0, seção missing-ticket-notification](../knowledge/nd-issues-005.md), caracteres [529, 1313).

> 1. Entre com sua própria conta no espaço correto e consulte o histórico em **Meus chamados**, conforme `nd-tickets-001`.
> 2. Confira se há uma mensagem que esperava acompanhar. O histórico é o local para ler o atendimento; o assistente não acessa esse registro em tempo real.
> 3. Abra **Preferências** e confira **Avisos de chamados por e-mail**, conforme `nd-service-003`. Se desejar avisos e estiverem desativados, ative e salve; não prometa envio retroativo.
> 4. Confira a caixa de entrada e a pasta de mensagens indesejadas do e-mail da própria conta. Não forneça sua senha de e-mail ao suporte.
> 5. Se a preferência estiver ativa e a dúvida persistir, encaminhe à revisão humana com o protocolo e uma descrição da mensagem esperada, sem copiar conteúdo privado ou dados de terceiros.

Revisão humana: **pending**.

### rag-test-011 — test — grupo `missing-ticket-notification`

**Pergunta:** Sem notificação na caixa de entrada significa que ninguém respondeu ao meu chamado?

**Gabarito proposto:** A ausência de e-mail não prova falta de resposta. Consulte o histórico do chamado na própria conta e espaço. Se não houver mensagem no histórico, não prometa que deveria ter sido enviado um aviso.

**Evidência:** [nd-issues-005, v1.0.0, seção scope](../knowledge/nd-issues-005.md), caracteres [75, 460).

> Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.
> 
> Aplica-se à ausência de um aviso por e-mail sobre mensagem em chamado próprio. Não se aplica a redefinição de senha ou convite. Não receber e-mail não prova que a equipe deixou de responder.

**Evidência:** [nd-issues-005, v1.0.0, seção missing-ticket-notification](../knowledge/nd-issues-005.md), caracteres [529, 1313).

> 1. Entre com sua própria conta no espaço correto e consulte o histórico em **Meus chamados**, conforme `nd-tickets-001`.
> 2. Confira se há uma mensagem que esperava acompanhar. O histórico é o local para ler o atendimento; o assistente não acessa esse registro em tempo real.
> 3. Abra **Preferências** e confira **Avisos de chamados por e-mail**, conforme `nd-service-003`. Se desejar avisos e estiverem desativados, ative e salve; não prometa envio retroativo.
> 4. Confira a caixa de entrada e a pasta de mensagens indesejadas do e-mail da própria conta. Não forneça sua senha de e-mail ao suporte.
> 5. Se a preferência estiver ativa e a dúvida persistir, encaminhe à revisão humana com o protocolo e uma descrição da mensagem esperada, sem copiar conteúdo privado ou dados de terceiros.

**Evidência:** [nd-issues-005, v1.0.0, seção exceptions](../knowledge/nd-issues-005.md), caracteres [1358, 1614).

> Se a pessoa não acessar mais o e-mail, não sugira entrar na conta de terceiros ou mudar o destinatário por um menu inexistente. Informe a limitação para revisão humana. Se não houver mensagem no histórico, não prometa que um aviso deveria ter sido enviado.

Revisão humana: **pending**.

### rag-test-012 — test — grupo `ticket-export-unavailable`

**Pergunta:** Como exporto todos os meus chamados em CSV?

**Motivo de abstenção:** Não há procedimento de exportação de chamados documentado.

**Resposta esperada:** A base não fornece informação suficiente. Não há procedimento de exportação de chamados documentado.

Revisão humana: **pending**.

### rag-test-013 — test — grupo `support-sla-unavailable`

**Pergunta:** Qual é o prazo máximo garantido para responderem meu chamado?

**Motivo de abstenção:** O corpus não define prazo garantido de atendimento ou conclusão.

**Resposta esperada:** A base não fornece informação suficiente. O corpus não define prazo garantido de atendimento ou conclusão.

Revisão humana: **pending**.

### rag-test-014 — test — grupo `account-erasure-unavailable`

**Pergunta:** Como apago definitivamente minha conta e todos os dados associados?

**Motivo de abstenção:** Não há procedimento de exclusão definitiva de conta e dados. Logout e pedido de saída de espaço não documentam essa exclusão.

**Resposta esperada:** A base não fornece informação suficiente. Não há procedimento de exclusão definitiva de conta e dados. Logout e pedido de saída de espaço não documentam essa exclusão.

Revisão humana: **pending**.

### rag-test-015 — test — grupo `tax-policy-unavailable`

**Pergunta:** Qual a alíquota de imposto aplicada à assinatura do NexoDesk?

**Motivo de abstenção:** A base não define tributos ou alíquotas da assinatura.

**Resposta esperada:** A base não fornece informação suficiente. A base não define tributos ou alíquotas da assinatura.

Revisão humana: **pending**.

## Verificação e pendências

Conferir JSON sem chaves duplicadas, contagens, IDs e perguntas únicos, separação de grupos, hashes dos arquivos e documentos, offsets e seções, contrato de abstenção e preservação do piloto. Verificações locais executadas com sucesso: 50 IDs e perguntas únicos, 22/11 grupos sem interseção, hashes, passagens/offsets dentro das seções, contratos de resposta/abstenção e revisão, links locais e preservação byte a byte do piloto e dos 24 Markdown. Os resultados estão registrados no diário. O conjunto não foi executado contra busca ou geração. Aceite da S1.8 exige revisão humana das 42 novas anotações e congelamento documentado do teste. S1.9 não iniciada.

## Conferência rápida dos snapshots

Na raiz do repositório, execute:

```bash
sha256sum configs/corpus_manifest.json evaluation/rag_development.json evaluation/rag_test.json
```

Compare o hash do corpus com `corpus_manifest_sha256` e os outros dois com `partitions[].sha256` em `evaluation/rag_split_manifest.json`. Isso verifica integridade de arquivos, não correção dos gabaritos. A revisão de cada afirmação deve usar as passagens dos lotes acima. O piloto histórico permanece associado ao seu corpus de origem.
