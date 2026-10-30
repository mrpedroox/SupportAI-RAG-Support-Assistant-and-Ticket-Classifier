# SupportAI — Plano de desenvolvimento

Data: 28/10/2026. Referência: AGENTS.md v2.0, lido integralmente.

**Status: plano aprovado; Sprint 1 em andamento.** S1.1–S1.6 concluídas, com aprovação explícita do conteúdo do piloto pelo usuário. S1.7 em andamento: primeiro lote de três novos documentos preparado, total 6/24; novos documentos aguardam revisão humana. Oito anotações do piloto aprovadas, licença pendente e manifesto draft. S1.8–S1.9 não iniciadas. Não há aplicação, ingestão autorizada ou modelo treinado.

## 1. Objetivo e ponto de partida

Construir um assistente de suporte que recupera procedimentos, sugere respostas com fontes e exige revisão humana. Em um segundo marco obrigatório, treinar e operar um classificador de categoria de chamados. Aprender a justificar a arquitetura, avaliar resultados e reproduzir versões faz parte da entrega.

Na inspeção inicial, o repositório continha README.md, .gitignore e AGENTS.md, sem aplicação ou ambiente de dependências. A branch era `main`, com um commit inicial; AGENTS.md estava não rastreado pelo Git. Esse estado foi preservado durante o planejamento. Após aprovação, o trabalho passou à branch da sprint, e em 29/10/2026 os arquivos das etapas anteriores foram registrados em commits locais, incluindo o AGENTS.md existente sem alteração.

## 2. Conceitos para acompanhar as decisões

| Conceito | Significado e exemplo no projeto |
| --- | --- |
| Arquitetura de sistemas | Define responsabilidades, dados, interfaces e comportamento diante de falhas. A API recebe a pergunta; a recuperação busca evidências; a geração redige; a persistência guarda o histórico. |
| Monólito modular | Uma aplicação com módulos de responsabilidades distintas. Permite aprender as fronteiras sem operar vários serviços. RAG e classificação compartilham o backend e podem ser usados independentemente. |
| Corpus | Conjunto autorizado de documentos que descreve os procedimentos do produto. É a fonte de conhecimento recuperável. |
| Chunk | Trecho de um documento, ligado à versão, seção e posição de origem. Deve permitir localizar o procedimento que sustenta uma resposta. |
| Embedding | Vetor que representa um texto para comparação de similaridade. Gerá-lo com um modelo existente não treina nosso classificador nem altera os pesos do LLM. |
| Retrieval | Seleção de trechos relevantes. A busca textual usa termos; a vetorial compara embeddings. Mediremos a textual como baseline antes da vetorial. |
| Contexto e geração | Contexto é o conjunto de evidências enviado ao LLM com a pergunta. Geração é a redação da sugestão. Uma busca boa ainda pode resultar em uma resposta incorreta. |
| Grounding e abstenção | A resposta precisa ser sustentada pelas evidências. Quando elas faltam ou conflitam, o assistente explica a limitação. Uma citação existente não comprova sustentação semântica. |
| Treinamento supervisionado | Aprende parâmetros a partir de textos rotulados. TF-IDF transforma textos em características; regressão logística aprende a associá-las às categorias. Na inferência, o pipeline já treinado prevê a categoria de um novo chamado. |
| Baseline | Referência simples para comparação: busca textual para recuperação e DummyClassifier para classificação. Complexidade adicional precisa demonstrar utilidade. |
| Desenvolvimento, validação e teste | Em RAG, desenvolvimento orienta ajustes e teste fica reservado. No classificador, treino ajusta o pipeline, validação escolhe configurações e teste avalia a candidata final. São conjuntos diferentes para tarefas diferentes. |
| Vazamento de dados | Informação indevida favorece a avaliação. Exemplos: indexar gabaritos RAG, separar paráfrases entre partições ou ajustar TF-IDF com dados de teste. |
| MLOps e LLMOps | Práticas para ligar dados, código, configuração, avaliação e operação. MLflow registra experimentos e traces; promoção manual escolhe uma versão; rollback restaura uma combinação anterior compatível. |

Exercício manual proposto para o piloto: ler um procedimento fictício sobre perda de autenticador; destacar a passagem necessária; escrever uma pergunta e uma resposta sustentada por ela. Depois perguntar algo ausente, como uma exceção de cobrança não documentada, e justificar a abstenção. Esse exercício antecipa recuperação e geração sem chamar modelos.

## 3. Arquitetura alvo e limites desta sprint

Fluxo previsto, ainda sem implementação:

```text
Ingestão local: Markdown autorizado → validação → chunks → embeddings → coleção

Consulta: atendente → FastAPI → recuperação → contexto → LLM → validação
                                                               ↓
                                           PostgreSQL: original + fontes
                                                               ↓
                                                  revisão autenticada

Classificação: chamados rotulados → treino/validação → pipeline versionado
                                                               ↓
                                           FastAPI → categoria + revisão

MLflow: experimentos, avaliações e traces sem conteúdo sensível por padrão
```

PostgreSQL guarda versões e histórico; pgvector permite busca vetorial. O classificador não filtra os documentos recuperados: um erro de categoria não deve esconder evidências. Ingestão e treinamento são comandos administrativos, separados das requisições de atendimento.

Nas sprints de implementação, falhas de fornecedor serão erros técnicos, distintos de falta de evidência. Respostas serão rascunhos com `requires_human_review=true`; o status factual será `answered` ou `insufficient_evidence`. O original será preservado após revisões. Não haverá envio automático ao cliente.

A Sprint 1 prepara dados e critérios de avaliação. API, banco, migrations, embeddings, geração, treino, MLflow em execução, Docker, CI e deploy ficam nas respectivas sprints posteriores. A preparação mínima de Python e dependências foi concluída na S1.2 após aprovação do plano.

## 4. Sprint 1 — Domínio, corpus e avaliação

**Problema:** sem delimitar o produto e preparar evidências, não saberemos distinguir uma resposta correta de uma resposta apenas convincente.

**Solução:** definir o domínio, validar um piloto pequeno e ampliar uma base fictícia coerente com perguntas anotadas, mantendo gabaritos fora da ingestão.

**Como verificar:** revisar manualmente a relação pergunta → passagem de origem → resposta esperada; conferir origem, versões, separação e contagens. Nesta sprint ainda não há métricas de desempenho de um sistema.

Branch da Sprint 1: `feature/s1-knowledge-foundation`, criada no início da execução. Uma sprint é um marco de aprendizado; a referência de 1–2 semanas depende da disponibilidade e não é compromisso de prazo. Cada tarefa pode ocupar uma ou mais sessões curtas.

### 4.1 Suposições reversíveis e decisões pendentes

- Domínio adotado após aprovação do plano: suporte a um produto SaaS fictício, provisoriamente chamado **NexoDesk**, em português. Cobertura: conta e acesso, uso básico, cobrança e solicitações de serviço. Procedimentos serão explicitamente fictícios. Limites e exemplos estão em DATA.md.
- Público: um atendente de uma única organização. Não haverá dados reais de clientes nem documentos privados no piloto.
- Meta de corpus: 24 documentos, dentro da faixa obrigatória de 20–40. Começar com 3 documentos antes de expandir.
- Meta de avaliação: 50 perguntas, aproximadamente 35 de desenvolvimento e 15 de teste, com grupos preservados. A proporção pode mudar ligeiramente para manter grupos íntegros; registrar contagens reais.
- Orçamento proposto para a Sprint 1: R$ 0 em APIs e hospedagem; trabalho local e anotação manual. Nenhum gasto foi realizado por este planejamento. O teto das próximas sprints será acordado antes de chamadas pagas.
- Domínio e cobertura registrados na S1.1 conforme o plano aprovado. Licença dos materiais autorais e do código continuam pendentes; caráter fictício não equivale a licença de redistribuição.
- Ambiente escolhido e verificado na S1.2: CPython 3.12.16, uv 0.13.6, pytest 9.2.0 e Ruff 0.17.3; um único uv.lock. A compatibilidade verificada cobre as ferramentas atuais, não a stack futura. Fornecedor, modelos, preços, retenção de dados e infraestrutura serão avaliados quando necessários, antes do uso externo.
- Categorias do classificador ainda não estão fixadas. A cobertura do corpus deve permitir discutir 4–6 categorias, mas a taxonomia final depende do piloto de chamados da Sprint 7.

### 4.2 Tarefas pequenas, em ordem

**S1.1 e S1.2 concluídas em 28/10/2026; S1.3 em 29/10/2026; S1.4 e S1.5 em 30/10/2026. S1.6 concluída por aprovação explícita do usuário em 30/10/2026, licença mantida pendente; S1.7 em andamento: lote 1 preparado, 6/24 documentos, 3 novos aguardando revisão humana; S1.8–S1.9 não iniciadas.** Dependências indicam o que deve ser revisado antes de avançar.

| ID | Problema e ação proposta | Entrega prevista | Como verificar |
| --- | --- | --- | --- |
| S1.1 | Delimitar o conhecimento: revisar o domínio proposto, usuário, situações cobertas e exclusões. | Seção de escopo em docs/DATA.md e breve ajuste no README. | Explicar 3 perguntas cobertas e 3 fora do escopo; não prometer procedimentos reais. |
| S1.2 | Tornar o ambiente reproduzível: escolher Python suportado, um gerenciador e dependências mínimas conforme necessidade. | Versão de Python fixada, pyproject.toml e um único lock; instruções verificadas. | Recriar o ambiente pelo lock e conferir a versão; não adicionar módulos vazios ou comandos fictícios. Depende de S1.1. |
| S1.3 | Tornar fontes auditáveis: definir metadados, manifesto autorizado e política de dados. | docs/DATA.md com origem, autoria, licença, idioma, caráter fictício, obtenção, atualização, retenção/exclusão e limitações; formato do manifesto. | Um documento deve ser identificável por ID, título, versão, fonte, checksum e seções; apenas caminhos autorizados entram no manifesto. Depende de S1.1. |
| S1.4 | Testar a coerência antes de escalar: redigir 3 procedimentos curtos com passos, condições e exceções. | Piloto em knowledge/ e entradas no manifesto. | Revisar manualmente contradições, integridade de passos e rastreabilidade; distinguir metadados do checksum calculado sobre o arquivo final. Depende de S1.3. |
| S1.5 | Testar a anotação: criar 8 perguntas de desenvolvimento sobre o piloto. | Piloto em evaluation/, com evidências ou motivo de abstenção. | Proposta: 6 respondíveis e 2 sem resposta; incluir paráfrase, abreviação e ambiguidade. Nenhuma referência pode exigir informação ausente. Depende de S1.4. |
| S1.6 | Corrigir ambiguidades do próprio conjunto: revisar documentos e perguntas com o usuário. Concluída: aprovação explícita do usuário registrada em docs/PILOT_REVIEW.md; licença pendente. | Piloto revisado e decisões registradas. | Percorrer manualmente pergunta, documento/versão, passagem e resposta. Material gerado por LLM permanece pendente até revisão humana. Depende de S1.5. |
| S1.7 | Em andamento: lote 1 com 3 novos documentos (6/24), roteiro em docs/S1_7_BATCH_REVIEW.md. Ampliar em lotes de 3–5, revisando cada lote. | Meta de 24 documentos; manifesto com caminhos e checksums, versão do corpus e contagens. | Todos com metadados completos, sem duplicatas contraditórias; manter entre 20 e 40. Depende do aceite do piloto S1.6. |
| S1.8 | Completar a avaliação: ampliar as perguntas em lotes de 5–10 e separar grupos antes de atribuir partições. | Cerca de 50 perguntas revisadas; manifestos de desenvolvimento/teste com IDs, grupos, versões e checksums. | Nenhum grupo em ambas as partições; perguntas respondíveis e sem resposta nas duas; evidências localizáveis. As 8 perguntas já usadas no piloto ficam em desenvolvimento. Depende de S1.7. |
| S1.9 | Registrar limites e preparar o próximo passo: auditar os dados e explicar o que ficou pronto. | README atualizado, entrada em docs/PROJECT_LOG.md e status neste plano. | Apresentar contagens, pendências, orçamento e exercício manual; encerrar a sprint apenas com todos os critérios atendidos. Depende de S1.8 e S1.2. |

Após cada unidade, explicar o que mudou, por que foi escolhido, o conceito aprendido, como foi verificado e o resultado observado. Revisar o piloto antes da expansão e fazer checkpoint no fim da sprint. A próxima unidade é S1.7. Preferência do usuário registrada em 29/10/2026: concluir cada unidade coesa com verificações e commit local em inglês no formato Conventional Commits; não deixar entregas sem commit. Push e merge continuam dependentes de autorização específica.

### 4.3 Contratos de dados propostos

**Documentos:** ID estável, título, versão, fonte, autoria/licença, idioma, data de atualização e seções identificáveis. O manifesto registra caminho autorizado e SHA-256 dos bytes do arquivo final; assim, alterações no conteúdo podem ser detectadas sem colocar o checksum dentro do próprio arquivo que ele identifica.

**Perguntas RAG:** `question_id`, `question`, `answerable`, `reference_answer`, `relevant_document_ids`, `evidence_spans`, `group_id`, `split`, `annotation_source` e `abstention_reason` quando aplicável. Registrar também a revisão humana, separadamente da origem da pergunta.

Cada `evidence_span` aponta para documento, versão, seção, passagem literal e offsets com convenção documentada, por exemplo intervalo de caracteres `[start, end)` no texto UTF-8 decodificado, sem normalização implícita. Conferir a passagem contra o snapshot. A correspondência com chunks será resolvida na Sprint 2; o gabarito não depende de IDs de chunks ainda inexistentes.

Para perguntas sem resposta, evidências ficam vazias e o motivo esperado de abstenção é explícito. Nas ambiguidades, registrar se as fontes permitem responder ou se falta informação para selecionar um procedimento.

**Agrupamento:** uma pergunta direta e sua paráfrase, como “Perdi o autenticador” e “Troquei de celular e perdi o 2FA”, permanecem no mesmo `group_id` e partição. Documentos podem aparecer como fonte nas duas partições; estamos avaliando consultas sobre uma base fixa.

**Uso do teste:** anotar e revisar o teste não é executar uma avaliação do modelo. Após a revisão, congelar sua versão e não usá-lo para escolher chunking, top-k, prompts ou limiares. Qualquer exposição que influencie decisões deve ser registrada. Trabalhando sozinho, declarar ausência de segundo anotador e reler uma amostra em outro momento.

**Chamados supervisionados:** serão preparados separadamente na Sprint 7, com `example_id`, `text`, `label`, `source`, `group_id`, `created_at` e versão da taxonomia. Não reutilizar perguntas RAG como se fossem um dataset rotulado de classificação. A referência futura é 500–1.500 exemplos revisados, sujeita à viabilidade e limitações documentadas; não gerar volume artificial para cumprir contagem.

### 4.4 Estrutura a criar gradualmente

```text
docs/PLAN.md                 # criado nesta sessão
docs/DATA.md                 # política e contratos dos dados
docs/PROJECT_LOG.md          # decisões, aprendizado e verificações
docs/PILOT_REVIEW.md         # roteiro do piloto e aprovação explícita do usuário
docs/S1_7_BATCH_REVIEW.md    # lote 1 de expansão para revisão humana
knowledge/                   # seis procedimentos: três aprovados, três pendentes
configs/corpus_manifest.json # seis entradas em draft, com checksums reais
evaluation/rag_dev_pilot.json # oito anotações aprovadas, snapshot do piloto; nunca indexar
pyproject.toml               # dependências e configuração mínima
uv.lock                      # único lock de dependências
.python-version              # CPython 3.12.16
```

A árvore representa a estrutura incremental. Documentação, ambiente, manifesto, seis documentos e oito perguntas existem; piloto aprovado pelo usuário e três novos documentos aguardando revisão humana. Os documentos não estão aprovados para ingestão. Usar caminhos relativos ao repositório nos manifestos para permitir reprodução em outra máquina. Criar módulos de aplicação apenas quando tiverem responsabilidade executável nas próximas sprints.

### 4.5 Critérios de aceite da Sprint 1

- [x] Domínio, usuário, cobertura, exclusões e orçamento inicial registrados e revisados conforme o plano aprovado.
- [x] Ambiente mínimo reproduzível, versão de Python fixada e um único lock; recriação offline em ambiente vazio com versões idênticas verificada na S1.2.
- [ ] 20–40 documentos revisados, com origem/licença ou caráter fictício identificados e situação de redistribuição documentada.
- [ ] Manifesto delimita fontes autorizadas e registra IDs, versões, checksums e contagens.
- [ ] Cerca de 50 perguntas com revisão humana, grupos e partições persistidos.
- [ ] Desenvolvimento e teste incluem perguntas respondíveis e sem resposta, com contagens por tipo.
- [ ] Toda resposta de referência é sustentada por passagens localizáveis; abstenções têm motivo.
- [ ] Nenhum grupo compartilhado entre partições; gabaritos separados do corpus e excluídos do manifesto.
- [ ] Teste congelado após anotação; uso prévio e limitação de revisão individual declarados.
- [ ] README e diário descrevem apenas verificações realizadas e pendências reais.
- [ ] Usuário consegue explicar corpus versus avaliação, retrieval versus geração e RAG versus treinamento supervisionado usando o piloto.

As metas de qualidade do AGENTS.md serão medidas nas sprints de avaliação: referências iniciais de 80% para Hit@5, respostas sem afirmações relevantes não sustentadas e abstenção em perguntas sem resposta, sempre com denominadores. Não são resultados desta sprint. Teto de latência e custo operacional será definido após medições do fornecedor escolhido, antes do teste final.

## 5. Caminho até o projeto completo

| Sprint | Entrega e aprendizado central | Estado |
| --- | --- | --- |
| 1 — Domínio, corpus e avaliação | Dados autorizados, perguntas revisadas, evidências e ambiente mínimo. | S1.1–S1.5 concluídas; S1.6 aprovada; S1.7 lote 1 preparado (6/24) |
| 2 — Ingestão e busca | PostgreSQL/pgvector, migrations, chunking, coleção idempotente e publicação atômica; comparar busca textual e vetorial no desenvolvimento, com MLflow inicial. | Não iniciada |
| 3 — Geração e API RAG | LLM, contexto limitado, fontes validadas, abstenção, persistência, autenticação e health checks. | Não iniciada |
| 4 — Revisão e avaliação | Histórico autenticado e concorrência, avaliação humana, segurança, tracing sanitizado, tokens e custo. | Não iniciada |
| 5 — Docker e CI | Ambiente reproduzível, testes sem chamadas pagas, integração pgvector, migrations e restauração local. | Não iniciada |
| 6 — Release RAG | Configuração candidata congelada, teste reservado, system card, demo verificado e rollback. Marco intermediário. | Não iniciada |
| 7 — Treinamento supervisionado | Taxonomia e chamados revisados; split por grupos 70/15/15 como referência; DummyClassifier versus TF-IDF + LogisticRegression em Pipeline; macro-F1 e métricas por classe; MLflow e model card. Teste reservado para candidata final. | Não iniciada |
| 8 — Serving e MLOps completo | Artefato aprovado com checksum, classificação/revisão na API, monitoramento, segunda rodada controlada de treino, comparação na validação, promoção manual, avaliação final, deploy atualizado e exercício de rollback. | Não iniciada |

O ciclo obrigatório do classificador será: dados e rótulos → treino → avaliação sem vazamento → registro → promoção manual → inferência → monitoramento → retreino controlado → rollback. A API carrega o pipeline aprovado; não treina durante requisições. A segunda rodada pode não melhorar o modelo, mas precisa ter comparação e decisão registradas.

O projeto completo exige os dois marcos. Resultados insuficientes devem ser publicados com diagnóstico e estado experimental, sem alegar aptidão para respostas ou roteamento autônomos.

## 6. Registro deste planejamento

- Inspeções realizadas: leitura integral das 656 linhas do AGENTS.md, README, .gitignore, arquivos presentes, estado e histórico recente do Git.
- Alteração desta sessão: criação de docs/PLAN.md.
- Implementação e testes de aplicação: não realizados; esta entrega é documental.
- Atualização após aprovação: S1.1 executada; DATA.md e PROJECT_LOG.md criados, README e status deste plano atualizados. A aplicação não foi implementada.
- Atualização S1.2: Python e uv fixados, dependências mínimas com lock, exclusões de arquivos locais e instruções verificadas no README. Um segundo ambiente vazio foi criado offline com versões idênticas, sem modificar uv.lock. Não há testes de aplicação a executar.
- Atualização S1.3: contrato de metadados e manifesto versão 1, formato de seções Markdown, regras de autorização e caminhos, atualização e retenção/exclusão documentados em DATA.md. Manifesto criado em draft com zero documentos; validação de ingestão fica para Sprint 2. Licença dos materiais e revisão humana permanecem pendentes.
- Atualização S1.4: três procedimentos fictícios em knowledge/ (recuperação de acesso, consulta de faturas e acompanhamento de chamados); manifesto 0.1.1-draft com checksums dos bytes finais, seções e datas reais. Condições e exceções revisadas pelo agente; licença e revisão humana continuam pendentes. Nenhum sistema foi implementado e nenhuma fonte está autorizada para ingestão.
- Atualização S1.5: evaluation/rag_dev_pilot.json contém oito perguntas (6 respondíveis e 2 sem resposta), em sete grupos, todas em desenvolvimento; evidências por passagem literal, versão, checksum, seção e offsets Unicode. Avaliação 0.1.0-draft, criada com assistência de LLM e revisão humana pendente. Nenhum teste reservado criado ou resultado de modelo alegado.
- Preparação S1.6: docs/PILOT_REVIEW.md reúne documentos completos por links, perguntas, gabaritos, passagens, interpretação das abstenções e snapshots com SHA-256. Conferência editorial do agente sem correção obrigatória identificada; isso não constitui aceite humano. Corpus, manifesto e avaliação não alterados.
- Para concluir S1.6: usuário revisa o material, informa aprovação ou correções por ID e identificação de revisor; registra-se também a decisão sobre licença, inclusive mantê-la pendente sem publicação. Só após a decisão real atualizar revisão e status; não iniciar S1.7 enquanto o aceite estiver pendente.

## Decisão S1.6 — 30/10/2026

Usuário aprovou os três documentos e as oito anotações e autorizou prosseguir para S1.7. Identificação operacional project-user; registro 2026-10-30T15:53:10Z. Licença mantida pendente, manifesto draft; sem segundo revisor humano independente. As descrições anteriores de preparação são históricas. Não houve alteração de procedimentos, perguntas ou evidências.

## Progresso S1.7 — lote 1 — 30/10/2026

Três documentos complementares sobre chamados preparados; total 6/24. Piloto preservado, licença pendente, novos textos ainda sem aprovação humana. Revisar docs/S1_7_BATCH_REVIEW.md antes do próximo lote. A avaliação mantém o snapshot de origem do commit 7acb503; S1.8 não iniciada.
