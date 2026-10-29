# AGENTS v2.0 — SupportAI: RAG, classificação e MLOps

Guia de trabalho para um projeto de portfólio que integra arquitetura de sistemas, RAG, LLMs, treinamento supervisionado, MLOps e LLMOps. Revisão de escopo em 28/10/2026: treinamento e operação do classificador passam a ser obrigatórios, preservando a entrega incremental do assistente RAG.

Este documento orienta a pessoa desenvolvedora e o agente que a acompanha. O objetivo é construir um sistema pequeno, reproduzível e demonstrável, entendendo as decisões ao longo do caminho. Não implemente todo o projeto de uma vez.

## 1. Papel e princípios

Atue como engenheiro de software com postura de mentor. O usuário está aprendendo: código funcionando e compreensão fazem parte da entrega.

- Explique em português; use inglês para identificadores, mensagens de commit e docstrings.
- Antes de uma etapa, apresente o problema, a solução proposta e como verificar o resultado.
- Prefira exemplos curtos e executáveis a longas explicações abstratas.
- Implemente uma fatia funcional por vez, com mudanças pequenas e coesas.
- Faça suposições reversíveis quando forem suficientes para avançar e registre-as. Pergunte quando a resposta afetar escopo, custo, exposição de dados ou uma decisão difícil de desfazer.
- Não introduza ferramentas apenas para aumentar a lista de tecnologias do currículo.
- Não invente resultados, testes executados, ganhos de modelo ou recursos de produção.
- Preserve mudanças existentes do usuário. Antes de editar, examine a estrutura, o estado do Git e as instruções aplicáveis ao repositório.

## 2. Problema e escopo do MVP

**Problema:** ajudar um atendente a encontrar procedimentos de suporte e elaborar uma resposta sustentada pela base de conhecimento, com fontes verificáveis e revisão humana.

**Núcleo principal:** um assistente RAG (*Retrieval-Augmented Generation*). O sistema recupera trechos de documentos e os fornece como contexto a um LLM para redigir uma sugestão de resposta. Não há treinamento ou fine-tuning de LLM no MVP.

**Segundo marco obrigatório:** treinar, avaliar, versionar e servir um classificador de categoria de chamados com TF-IDF + regressão logística, integrado à API. RAG e classificação são duas capacidades do mesmo produto e compartilham o backend. O projeto só está concluído quando os dois marcos forem entregues; terminar o RAG é uma entrega intermediária.

A formação em MLOps deve cobrir o ciclo completo: dados e rótulos → treino → avaliação sem vazamento → registro → promoção manual → inferência → monitoramento → retreino controlado → rollback. Gerar embeddings ou ajustar prompts não substitui treinamento supervisionado.

Exemplo ilustrativo, dependente da base disponível:

```text
Pergunta: “Troquei de celular e perdi acesso ao autenticador.”
Busca: recupera trechos do procedimento de recuperação de acesso.
Sugestão: descreve os passos sustentados pelos trechos recuperados.
Fontes: documento, versão, seção e identificador do trecho.
Status: draft; precisa de revisão do atendente antes de envio.
```

Quando não houver evidência suficiente, a resposta deve informar a limitação e pedir contexto ou encaminhar à revisão. Não completar procedimentos com detalhes inventados. Ter citações não comprova que uma resposta esteja correta: avalie se elas realmente sustentam as afirmações.

### Incluído

- Um domínio de suporte delimitado, em português, com 20 a 40 documentos públicos ou fictícios em Markdown.
- Ingestão por comando administrativo local; divisão em trechos, metadados, embeddings e indexação.
- Busca textual como baseline e busca semântica com PostgreSQL + pgvector.
- Um único provedor de LLM e um único modelo de embeddings por versão do índice.
- Uma pergunta independente por requisição, resposta com fontes ou abstenção explícita.
- FastAPI, PostgreSQL, migrations, histórico de respostas e feedback/correção humana autenticados.
- Avaliação de recuperação e geração, tracing com MLflow, latência, tokens e estimativa de custo.
- Dataset rotulado de chamados, baseline trivial e treinamento TF-IDF + regressão logística.
- Avaliação por classe, artefato versionado, promoção manual, inferência, correção de categoria, monitoramento e uma rodada controlada de retreino.
- Testes, Docker Compose, CI e deploy de demonstração.

### Fora do MVP

- Previsão aprendida de prioridade e equipe. O classificador obrigatório prevê apenas categoria; roteamento por regras pode ser extensão.
- Chat com memória, agentes autônomos, execução de ferramentas e envio automático ao cliente.
- Fine-tuning de LLM, treinamento de embeddings e múltiplos provedores simultâneos.
- PDFs complexos, OCR, scraping aberto e upload público de arquivos.
- Busca híbrida, reranking, expansão de consultas e índices aproximados antes de medir o baseline.
- Kafka, Kubernetes, microsserviços e pipelines automáticos de retreinamento.
- Frontend próprio obrigatório: OpenAPI e exemplos de chamadas bastam para demonstrar.

## 3. Arquitetura e fluxo

```text
Ingestão administrativa, fora do caminho da consulta:
Markdown autorizado → validação → chunks + metadados → embeddings
                                                         ↓
                                       PostgreSQL + pgvector
                                       coleção versionada

Consulta:
Atendente → FastAPI → busca de trechos → montagem de contexto → LLM
                         ↓                     ↓                ↓
                  IDs e scores         limites de tokens    validação
                                                               ↓
                          PostgreSQL ← resposta + fontes + status
                               ↑
                       revisão do atendente

MLflow: rastreia etapas e avaliações RAG sem expor conteúdo sensível.

Trilha supervisionada obrigatória, após a primeira entrega RAG:
Chamados rotulados → split por grupos → treino/validação → MLflow
       → artefato aprovado → API de classificação → PostgreSQL
       → revisão humana → dados curados → retreino manual e comparação
```

**Conceitos a ensinar:** embedding representa texto em vetor; chunk é um trecho recuperável; retrieval seleciona evidências; contexto é o material enviado ao LLM; geração produz a resposta. RAG não implica alterar os pesos do modelo.

Separe ingestão, recuperação, geração, classificação e persistência em módulos pequenos. O classificador sugere categoria, mas não filtra a recuperação RAG no escopo inicial: um erro de triagem não deve esconder evidências úteis. Ambos podem ser usados independentemente. Use funções e serviços explícitos; não crie camadas genéricas sem necessidade. Comece com SDKs e SQL/ORM diretos. LangChain ou LlamaIndex são opcionais depois que o fluxo básico estiver compreendido e houver benefício concreto.

### Stack mínima

| Responsabilidade | Escolha |
| --- | --- |
| Linguagem | Uma versão suportada de Python, fixada no projeto |
| API e validação | FastAPI e Pydantic |
| Banco e busca vetorial | PostgreSQL com extensão pgvector |
| Acesso e migrations | SQLAlchemy e Alembic |
| Baseline de recuperação | Full-text search do PostgreSQL, com configuração de idioma documentada |
| Embeddings e geração | Um modelo de embeddings adequado ao português e um LLM via SDK |
| Treinamento supervisionado | scikit-learn: DummyClassifier e Pipeline TF-IDF + LogisticRegression |
| Experimentos e tracing | MLflow local inicialmente |
| Qualidade | pytest e Ruff para lint e formatação |
| Dependências | `pyproject.toml` e um único arquivo de lock |
| Execução e CI | Docker Compose e GitHub Actions, se usar GitHub |

Escolha modelos, versões e fornecedor na implementação, registrando licença, limites, orçamento e política de dados. Comece com embeddings via API para reduzir requisitos locais; um modelo local pequeno é alternativa se custo, privacidade ou infraestrutura justificarem. Não adicione ambas as opções ao MVP por padrão.

Use timeouts explícitos e limite de concorrência para chamadas externas. Acesso síncrono é aceitável no início, desde que respeite o orçamento de latência; não bloqueie o event loop com SDK síncrono dentro de uma rota async sem tratamento adequado.

## 4. Base de conhecimento e conjunto de avaliação

### 4.1 Corpus

Escolha um cenário delimitado, como procedimentos de acesso e uso de um produto fictício. Use 20 a 40 documentos curtos com regras coerentes, passos, exceções e datas/versões identificadas. Não use procedimentos reais sensíveis ou contraditórios sem curadoria.

Registre em `docs/DATA.md` origem, licença, idioma, autoria, caráter fictício, data de obtenção, política de atualização e limitações. Markdown fictício e seguro pode ser versionado no Git; documentos privados, extrações brutas e credenciais ficam fora dele.

Cada documento deve possuir ID estável, título, versão, checksum, fonte e seções. Cada trecho deve referenciar documento/versão/seção, posição no texto e configuração de chunking. IDs ou offsets devem permitir localizar a evidência original.

### 4.2 Chunking e embeddings

- Comece separando por títulos e parágrafos; preserve passos de procedimentos sempre que possível.
- Referência inicial: 300 a 600 tokens por trecho, com sobreposição de 50 a 100 quando necessária. Ajuste na validação; não trate esses valores como regra universal.
- Use o tokenizer apropriado ou documente a aproximação; tokens não são caracteres.
- Prefixe contexto de título/seção quando isso ajudar a preservar o significado.
- Registre modelo/revisão de embeddings, dimensão, normalização, configuração e versão do pipeline.
- Use embeddings compatíveis para consulta e documentos, respeitando eventuais modos distintos do fornecedor. Não misture espaços vetoriais de modelos diferentes.
- Ao trocar modelo ou dimensão, gere outra coleção. Ao mudar chunking, gere nova versão do índice.

### 4.3 Avaliação anotada

Prepare cerca de 50 perguntas revisadas manualmente, com referência inicial de 35 para desenvolvimento e 15 para teste reservado. O tamanho é pequeno: publique contagens e limitações, sem alegar precisão estatística ampla.

Inclua perguntas diretas, paráfrases, termos abreviados, perguntas ambíguas e perguntas sem resposta na base. Tenha exemplos de abstenção em ambas as partições. Testes de prompt injection ficam também em uma suíte de segurança específica.

Campos sugeridos:

```text
question_id, question, answerable, reference_answer,
relevant_document_ids, evidence_spans, group_id, split, annotation_source
```

`evidence_spans` referencia documento/versão e passagem de origem; resolva sua correspondência com chunks em cada experimento para não invalidar o gabarito ao alterar chunking. Para perguntas sem resposta, liste o motivo esperado de abstenção em vez de fabricar evidências.

Agrupe paráfrases e perguntas derivadas do mesmo template na mesma partição. Perguntas geradas por LLM precisam de revisão humana e identificação de origem. Revisão independente é desejável; trabalhando sozinho, releia uma amostra em outro momento e declare essa limitação.

### 4.4 Prevenção de vazamento em RAG

O corpus conter a resposta é esperado em RAG. O que não pode ocorrer é indexar o arquivo de avaliação com perguntas, gabaritos, saídas do teste ou feedback não curado.

- Indexe apenas fontes explicitamente autorizadas no manifesto; não indexe o repositório inteiro.
- Escolha prompts, top-k, chunking e limiares no conjunto de desenvolvimento.
- Reserve o teste para a versão candidata final; registre qualquer uso anterior que tenha influenciado decisões.
- Use o mesmo snapshot do corpus para comparações. Persistir IDs e checksums evita comparar índices diferentes sem perceber.
- Não exija que documentos sejam disjuntos entre desenvolvimento e teste para avaliar consultas sobre uma base fixa. Se a pergunta for generalização para documentos novos, crie uma avaliação separada com esse objetivo explícito.
- Não use a resposta de referência como contexto de geração nem como entrada da busca.

### 4.5 Dataset obrigatório do classificador

Mantenha os dados supervisionados separados do corpus RAG e de seus gabaritos. Use chamados públicos com licença adequada ou fictícios identificados. Trabalhe com 4 a 6 categorias coerentes, por exemplo `access`, `billing`, `technical_issue`, `service_request` e `other`. Confirme compatibilidade com as fontes antes de fixar a taxonomia.

Referência de esforço: 500 a 1.500 exemplos revisados, buscando cerca de 100 por classe quando viável. Não fabrique exemplos para atingir uma contagem; reduza a taxonomia se necessário e documente a limitação. Resultados apenas sintéticos não demonstram desempenho em chamados reais.

Contrato mínimo: `example_id`, `text`, `label`, `source`, `group_id`, `created_at` (nulo quando desconhecido) e versão da taxonomia. Remova dados pessoais e campos que revelem a categoria, como equipe atribuída ou resolução posterior. Dados brutos e privados ficam fora do Git.

Defina inclusão/exclusão de cada classe e exemplos de fronteira. Faça um piloto de rotulagem e revise uma amostra novamente; informe quando não houver segundo anotador independente. Uma sugestão do modelo não vira rótulo humano automaticamente.

Agrupe conversas, duplicatas, paráfrases e famílias de templates antes da divisão. Comece com treino/validação/teste de 70/15/15, seed fixa e distribuição de classes aproximada, respeitando grupos. Quando o objetivo for prever chamados futuros e houver datas confiáveis, prefira corte temporal sem grupos compartilhados. Persistir os IDs, checksums, contagens e versão da taxonomia é obrigatório.

Ajuste TF-IDF e quaisquer transformações aprendidas somente no treino, dentro de um `Pipeline`. Em validação cruzada, preserve grupos e refaça o ajuste em cada fold. Selecione hiperparâmetros e limiar de revisão na validação; mantenha o teste reservado para a candidata final. Não use o teste RAG como teste de classificação: são tarefas e conjuntos diferentes.

## 5. Ingestão, recuperação e geração

### 5.1 Ingestão versionada e idempotente

Faça a ingestão por comando local administrativo, não por endpoint público. Valide formato, tamanho, licença e metadados; rejeite conteúdo vazio ou inválido com relatório.

Identifique execuções por versão do corpus, chunking e embeddings. Repetir a ingestão do mesmo snapshot não deve duplicar chunks ou cobranças de embeddings desnecessariamente; reaproveite resultados por hash quando seguro.

Construa uma coleção em estado `building`, valide contagens e dimensões e só então marque-a `ready` e publique sua referência ativa atomicamente. Falha parcial não pode substituir a coleção em uso. Cada consulta fixa uma coleção no início e usa essa versão até terminar.

Documentos alterados ou removidos não devem deixar chunks antigos acessíveis na coleção nova. Preserve uma versão anterior para rollback pelo prazo definido, respeitando exclusões de dados pessoais. Evite grandes transações abertas durante chamadas externas.

### 5.2 Baselines de recuperação

1. Implemente busca textual e registre resultados no conjunto de desenvolvimento.
2. Implemente busca vetorial exata por similaridade de cosseno com pgvector.
3. Compare top-k e resultados sob o mesmo corpus e orçamento de contexto.

Comece com `top_k=5` configurável e ajuste com evidência. Para o corpus pequeno, a busca exata basta inicialmente. Índices HNSW/IVFFlat, busca híbrida e reranking são extensões condicionadas a uma necessidade medida.

Scores de similaridade não são probabilidades de resposta correta. Documente se o valor retornado é distância ou similaridade e seu sentido de ordenação. Um limiar pode bloquear consultas sem evidência, mas não garante que todos os trechos acima dele sejam suficientes.

### 5.3 Contexto e resposta

- Construa contexto com IDs de fonte, trechos e separação explícita entre instruções e dados.
- Reserve orçamento para instruções, pergunta e saída; selecione/trunque trechos de modo determinístico sem ultrapassar o limite do modelo.
- Instrua o LLM a responder apenas com base nas evidências e reconhecer insuficiência ou conflito.
- Use saída estruturada quando suportada; valide o resultado no servidor em qualquer caso.
- Formato mínimo: `status` (`answered` ou `insufficient_evidence`), `answer` e `cited_chunk_ids`.
- Para `answered`, exija pelo menos uma citação válida dos trechos efetivamente recuperados e enviados ao modelo. Resolva título, versão e localização no servidor, sem confiar em URLs inventadas pelo LLM.
- Rejeite referências inexistentes. IDs válidos não garantem sustentação semântica: isso também precisa ser avaliado.
- Para `insufficient_evidence`, devolva uma explicação breve e, quando útil, uma pergunta de esclarecimento. Não apresente uma solução não sustentada como alternativa.
- Todas as respostas são rascunhos: `requires_human_review=true`, inclusive quando há evidência.

Falha do provedor, timeout e saída inválida são erros técnicos, diferentes de ausência de evidência. Não os converta em uma resposta factual nem em abstenção bem-sucedida. Uma correção de formato pode ter no máximo uma nova tentativa, dentro do orçamento global.

## 6. Backend, banco e revisão humana

### 6.1 Contratos mínimos

| Método e rota | Comportamento |
| --- | --- |
| `POST /v1/queries` | Busca, gera/abstém e persiste resultado; retorna `201` |
| `GET /v1/queries/{id}` | Retorna pergunta, resposta original, fontes e revisão vigente |
| `POST /v1/queries/{id}/reviews` | Confirma, corrige ou rejeita a resposta, com histórico; retorna `201` |
| `POST /v1/classifications` | Classifica o texto de um chamado e persiste sugestão/versionamento; retorna `201` |
| `GET /v1/classifications/{id}` | Consulta sugestão original e categoria vigente após revisão |
| `POST /v1/classifications/{id}/reviews` | Confirma/corrige categoria sem alterar a sugestão original; retorna `201` |
| `GET /health/live` | Indica processo ativo sem consultar dependências |
| `GET /health/ready` | Verifica banco, configuração e existência de coleção ativa pronta |

Entrada: `question` com 1 a 5.000 caracteres após trim. Rejeite apenas espaços e limite o tamanho total da requisição. Não aceite que o cliente escolha coleção privada, prompt de sistema, SQL ou URL para buscar conteúdo.

Resposta: `query_id`, `status`, `answer`, `sources`, `requires_human_review`, `collection_version`, `prompt_version`, `embedding_model`, `generation_model`, `created_at`. Cada fonte inclui chunk, documento, versão, seção e trecho exibível autorizado. Tokens e custo podem ficar em metadados operacionais internos.

Use UUIDs e datas UTC com timezone. Retorne `422` para entrada inválida, `401/403` para autenticação/permissão, `404` para recurso inexistente, `409` para revisão desatualizada, `429` para limites, `503` para dependência indisponível e `504` para timeout. Saída malformada do provedor após tratamento retorna erro de gateway, como `502`, sem expor detalhes internos.

Na primeira entrega, readiness exige a coleção RAG. Na release completa, exige também o classificador aprovado carregado; antes disso, os endpoints de classificação ainda não disponíveis retornam `503`. Registre separadamente a saúde das capacidades para diagnóstico.

Readiness não faz uma chamada paga ao LLM em cada sondagem. Diferencie prontidão local de disponibilidade do fornecedor, observada por chamadas reais e smoke test controlado. Na ausência de coleção ativa pronta, a API de consulta retorna `503`.

A criação de classificação recebe `text` com os mesmos limites de tamanho da pergunta e retorna `classification_id`, `predicted_category`, `score`, `needs_review`, `model_version`, `taxonomy_version` e data. O score máximo de `predict_proba` não é garantia de acerto nem probabilidade necessariamente calibrada. Baixo score mantém a categoria sugerida e sinaliza revisão; não vira `other` automaticamente.

A consulta distingue `predicted_category` de `effective_category`. Revisões de categoria exigem rótulo válido e usam as mesmas regras de autenticação, autoria no servidor e concorrência por `expected_revision` das revisões de resposta. Carregue o pipeline completo no início do processo; não consulte MLflow nem ajuste o modelo a cada requisição.

### 6.2 Persistência

Tabelas sugeridas:

- `documents` e `document_versions`: origem, título, licença, checksum e versão.
- `collections`: manifesto, configuração, modelo de embeddings, dimensão e estado.
- `chunks`: coleção, versão do documento, posição, texto, metadados e vetor.
- `queries`: pergunta sanitizada, estado de execução, resposta original/status, versões, revisão vigente e timestamps.
- `query_sources`: chunks recuperados, posição, score, inclusão no contexto e indicação de citação.
- `reviews`: query, decisão (`approved`, `edited`, `rejected`), texto corrigido quando aplicável, observação, revisor e número da revisão.
- `classifications`: texto sanitizado, categoria/score originais, necessidade de revisão, versão do modelo/taxonomia, revisão vigente e timestamps.
- `classification_reviews`: classificação, categoria confirmada/corrigida, revisor, justificativa e número da revisão.

Persista os IDs/versões suficientes para reconstruir quais evidências foram utilizadas. Histórico não pode apontar silenciosamente para o conteúdo novo de um documento alterado. Preserve snapshots autorizados pelo prazo definido; se houver exclusão, informe indisponibilidade da fonte histórica.

Não mantenha transação do banco aberta durante a chamada de rede ao LLM. Registre uma execução `pending`, execute as etapas externas e finalize atomicamente resposta e referências; em erro, registre `failed` com código sanitizado. Retorne sucesso apenas após persistência completa. Documente que uma repetição do POST pode gerar novo custo; idempotência de consultas pode ser uma melhoria posterior.

### 6.3 Feedback e correção

Preserve a resposta original. Exponha separadamente resposta gerada, revisão vigente e texto aprovado/corrigido. Rejeição não torna a resposta original válida para envio. Não envie mensagens automaticamente.

Exija identidade do revisor autenticada no servidor e `expected_revision`. Verifique/incremente a revisão e grave o registro na mesma transação; uma atualização concorrente desatualizada recebe `409`.

Feedback não modifica prompt, documentos ou índice automaticamente. Faça curadoria antes de incorporá-lo à base ou ao conjunto de avaliação; não contamine o teste reservado.

## 7. Avaliação de RAG

Avalie recuperação e geração separadamente para localizar falhas. Fixe corpus, conjunto de perguntas e configurações comparáveis.

### Recuperação

- **Hit@k:** fração das perguntas respondíveis com ao menos uma evidência relevante nos k primeiros resultados.
- **Recall@k:** fração das unidades de evidência relevantes recuperadas, com unidade e correspondência documentadas.
- **MRR:** posição da primeira evidência relevante, como métrica complementar.
- Analise perguntas sem resultado útil e diferenças entre busca textual e vetorial.

Não calcule recall sobre perguntas sem evidência esperada. Informe denominadores, k, contagens e exemplos. Ao mudar chunking, use o gabarito por passagem para evitar premiar simplesmente a fragmentação dos documentos.

### Geração e abstenção

Use uma rubrica simples de revisão humana por resposta:

- Correção em relação à resposta de referência e ao procedimento vigente.
- Sustentação das afirmações pelas fontes citadas, sem passos inventados.
- Relevância e cobertura das informações necessárias.
- Citações válidas e semanticamente pertinentes.
- Abstenção em perguntas sem resposta e abstenções indevidas em perguntas respondíveis.

Relate taxas e contagens de abstenção correta/incorreta separadamente; recusar tudo não é uma boa solução. Avaliação por LLM pode auxiliar, com prompt/modelo versionados, mas deve ser comparada com amostra humana e não ser a única evidência.

### Operação e comparação

Registre latência p50/p95 total e por etapa, erros, tokens de entrada/saída, embeddings de ingestão/consulta e custo estimado. Documente modelo, preços/data usados, carga, tamanho do corpus e tamanho da amostra. Marque contagens indisponíveis como desconhecidas, não zero.

Compare inicialmente busca textual + LLM versus vetorial + o mesmo LLM e prompt. Uma chamada ao LLM sem retrieval pode ser um controle opcional com documentos fictícios, deixando claro que isso não prova ausência de memorização em outras bases.

Defina metas no fim do piloto, antes do teste final. Referência inicial de desenvolvimento: Hit@5 de pelo menos 80% nas perguntas respondíveis, pelo menos 80% das respostas revisadas sem afirmações relevantes não sustentadas e pelo menos 80% de abstenções nas perguntas sem resposta. São metas de projeto, não garantias; publique valores exatos e denominadores pequenos. Defina também teto de custo e latência após medir o fornecedor escolhido.

Se a busca vetorial não superar a textual, registre o resultado e investigue. Se as metas falharem, entregue diagnóstico e mantenha o estado como experimental; não declare o assistente confiável para respostas autônomas.

### Avaliação obrigatória do classificador

Compare `DummyClassifier` com estratégia documentada, como classe mais frequente, e TF-IDF + regressão logística. Use uma grade pequena de regularização e `class_weight` quando necessário, sem busca extensa. Preserve acentos e negações inicialmente; n-gramas de palavras e de caracteres podem ser comparados na validação.

Métrica principal: **macro-F1**. Registre precisão, recall, F1 e suporte por classe, matriz de confusão, acurácia como contexto, latência p50/p95 e tamanho do artefato. Analise erros e desbalanceamento. Compare sempre as mesmas partições e informe contagens.

Escolha um limiar de revisão pela relação cobertura versus qualidade na validação. Informe cobertura e desempenho no subconjunto de alta confiança separadamente do resultado global. Não escolha o limiar no teste e não trate o score como probabilidade calibrada sem avaliação específica.

Meta inicial: superar o baseline trivial em macro-F1 e explicar os principais erros. Defina pisos por classe ao concluir o piloto. Se não houver ganho ou alguma classe crítica falhar, registre o diagnóstico e mantenha o classificador experimental, com revisão obrigatória; não publique uma promoção automática nem alegue aptidão para roteamento autônomo.

## 8. Segurança, privacidade e controle de custo

- Use corpus público ou fictício no demo. Não envie conteúdo privado a APIs externas sem autorização e entendimento da política de retenção do fornecedor.
- Armazene segredos em variáveis de ambiente; mantenha `.env.example` sem valores reais e ignore `.env`.
- Autentique consultas, leitura de histórico e revisões, com permissões distintas para revisor. O MVP é de um único tenant; múltiplos tenants exigem isolamento de documentos e filtros antes da recuperação, não apenas no prompt.
- Trate pergunta e documentos recuperados como dados não confiáveis. Instruções neles não podem alterar regras do sistema, revelar segredos ou autorizar ações.
- Não forneça ferramentas, shell ou acesso de escrita ao LLM. Não coloque credenciais no prompt.
- Delimitadores e instruções de sistema ajudam, mas não garantem resistência a prompt injection. Teste ataques diretos e instruções maliciosas dentro de documentos.
- Não faça fetch de URLs fornecidas pelo usuário. Fontes e caminhos de ingestão devem estar autorizados; bloqueie leitura fora da pasta permitida.
- Restrinja permissões do banco, use SQL parametrizado, HTTPS e limites de requisição. Não exponha PostgreSQL ou MLflow na internet.
- Defina tamanho máximo do contexto, tokens máximos de saída, timeout total, concorrência e limite de requisições por cliente. Retentativas para falhas transitórias devem ser limitadas e respeitar o orçamento.
- Configure um orçamento de demo e limite de gastos no fornecedor quando disponível. Não dependa exclusivamente de estimativas do código para impedir cobranças.
- Carregue artefatos serializados apenas de origem controlada pelo projeto, com checksum verificado. Nunca aceite upload de modelos de clientes; formatos como pickle/joblib podem executar código ao carregar.
- Logs e traces não armazenam pergunta, resposta ou contexto completos por padrão. Configure a instrumentação para evitar captura automática desses campos. Payloads completos só em fixtures públicas/fictícias ou com política explícita.
- Defina retenção e exclusão de perguntas, revisões, chunks, traces e backups. Ter histórico não justifica reter dados pessoais indefinidamente.

## 9. MLOps, LLMOps e MLflow com pouca burocracia

### Rastreabilidade do RAG

A unidade reproduzível é a configuração do sistema, não apenas o nome do LLM. Cada experimento relevante registra:

- Commit e indicação de mudanças locais não commitadas.
- Versões/checksums do corpus, coleção e conjunto de avaliação.
- Chunking, top-k, filtros, limiares e método de busca.
- Modelo/revisão de embeddings, dimensão e modelo de geração.
- Prompt versionado, parâmetros de geração e orçamento de tokens.
- Métricas de recuperação, geração, abstenção, latência e custo.
- Limitações, exemplos sanitizados e avaliação humana.

Traceie ingestão/embedding quando útil e etapas de consulta (`retrieval`, montagem de contexto, geração e validação). Correlacione `request_id` e trace ID. A indisponibilidade do tracking não deve derrubar consultas; registre a perda de telemetria sem conteúdo sensível.

Comece com MLflow local e um manifesto de release. Não exija servidor dedicado ou registry remoto. Modelos hospedados podem mudar: fixe revisões quando o fornecedor permitir e declare a limitação quando não permitir. Temperatura baixa não garante reprodução exata.

A promoção é manual: comparar no desenvolvimento, verificar testes, avaliar a candidata no conjunto reservado e publicar imagem + configuração + coleção identificadas. Rollback seleciona uma combinação anterior compatível; não basta trocar o prompt se o modelo de embeddings ou o schema mudou.

### Ciclo obrigatório do classificador

Cada execução relevante de treino deve registrar no MLflow commit/estado do código, seed, versões das dependências, parâmetros, checksum/versão do dataset e split, taxonomia, métricas, matriz de confusão e artefato completo do pipeline. Identifique a partição de cada métrica; registre teste apenas para a candidata final. Use exemplos sanitizados para assinatura e smoke test.

Um manifesto de release liga `model_version`, run ID, checksum do artefato, versão dos dados, taxonomia, ambiente e decisão de promoção. Um registry remoto é opcional; o registro rastreável dos experimentos e artefatos é obrigatório. Armazene o artefato fora do Git e empacote-o na imagem ou distribua-o de maneira controlada.

**Promoção manual:** treinar → comparar na validação → inspecionar erros e métricas por classe → avaliar candidata final → executar testes de integração → aprovar manifesto → implantar artefato imutável. A API não treina e não seleciona um caminho mutável chamado apenas `latest`.

**Retreino demonstrado:** faça ao menos uma segunda rodada com uma versão curada de dados ou melhoria justificada de configuração. Registre a mudança, compare a candidata ao modelo vigente nas mesmas condições e decida manter ou promover. Não é obrigatório haver ganho. Feedback deve ser revisado e anonimizado, com deduplicação contra as partições; não incorpore ao treino exemplos equivalentes aos grupos de teste. A candidata de retreino é comparada primeiro na validação; preserve o teste para avaliação da release candidata escolhida, sem transformá-lo em painel de experimentos.

**Monitoramento mínimo:** volume, falhas, latência, distribuição de categorias e scores, taxa de revisão e tamanho da fila de feedback. Mudança de distribuição é um sinal de investigação, não prova de queda de qualidade. Quando houver rótulos humanos suficientes, calcule métricas em uma amostra documentada e reporte o atraso de rotulagem. Taxa de correção entre revisados não é taxa de erro global.

**Rollback demonstrado:** guarde a versão anterior e valide, em ambiente local/staging, que voltar ao artefato e à configuração compatíveis restaura a inferência. Registre o teste no diário. Se a nova candidata não for promovida, simule a troca em staging sem alterar a release pública. Verifique compatibilidade de taxonomia e schema, além da versão do modelo.

Não introduza orquestrador ou retreino agendado para cumprir esses objetivos. Comandos explícitos, MLflow, manifesto e uma decisão humana bastam.

## 10. Estrutura do repositório

Crie pastas conforme forem necessárias; não gere módulos vazios para simular arquitetura.

```text
.
├── AGENTS.md
├── README.md
├── pyproject.toml
├── <arquivo-de-lock>
├── .env.example
├── .gitignore
├── Dockerfile
├── compose.yaml
├── .github/workflows/ci.yml
├── configs/                     # ingestão, retrieval e modelos
├── prompts/                     # templates versionados
├── knowledge/                   # Markdown público/fictício autorizado
├── evaluation/                  # perguntas/gabaritos RAG; nunca indexar
├── data/                        # chamados brutos ignorados; manifestos seguros versionados
├── models/                      # artefatos locais ignorados pelo Git
├── src/support_assistant/
│   ├── api/                     # rotas, schemas e autenticação
│   ├── db/                      # persistência e modelos
│   ├── ingestion/               # leitura, chunking e publicação de coleção
│   ├── retrieval/               # busca textual e vetorial
│   ├── generation/              # cliente LLM, contexto e validação
│   ├── evaluation/              # métricas e rubrica
│   ├── classification/          # dados, treino, avaliação e inferência supervisionada
│   └── settings.py
├── migrations/
├── tests/
│   ├── unit/
│   ├── integration/
│   ├── security/
│   └── fixtures/
├── scripts/                     # ingestão, avaliação e smoke test
└── docs/
    ├── PLAN.md
    ├── DATA.md
    ├── SYSTEM_CARD.md
    ├── MODEL_CARD.md             # classificador obrigatório
    └── PROJECT_LOG.md
```

## 11. Testes, Docker, CI, deploy e logs

### Testes relevantes

- Unitários: chunking com metadados, limites de contexto, ordenação dos resultados, validação da saída, citações inexistentes e cálculo das métricas com exemplos conhecidos.
- Integração com PostgreSQL + pgvector reais: migrations, dimensão dos vetores, busca com vetores fixos conhecidos, ingestão idempotente, publicação atômica e ausência de chunks obsoletos.
- API: consulta respondida, abstenção, histórico, aprovação/correção/rejeição e conflito de revisão, autenticação e erros de dependência.
- Segurança: prompt injection direto/indireto, acesso sem permissão, path traversal na ingestão e ausência de dados sensíveis em logs/traces.
- Provedor: clientes falsos para timeout, rate limit, resposta malformada e citações inventadas. Um teste determinístico com mock valida os controles, não a resistência real do LLM; inclua ataques também na avaliação manual/live controlada.

Para o classificador, teste schema e classes dos dados, ausência de sobreposição de IDs/grupos, carregamento do pipeline salvo, consistência da inferência, limiar de revisão e histórico/concorrência das correções. Verifique o checksum antes de carregar o artefato. Use dataset fictício mínimo na CI para smoke de treino/serialização; métricas dele não representam qualidade real. Treino completo e comparação de candidatas são comandos separados.

Não use SQLite como substituto dos testes de pgvector. Testes comuns não precisam de internet, GPU, chave real ou chamada paga. Um smoke test live separado e autorizado valida a integração real. Não declare testes executados quando apenas os escreveu.

### Docker e CI

Compose inicia API e PostgreSQL com pgvector habilitado, volume persistente e health checks. A ingestão é um comando separado: subir o ambiente não deve disparar chamadas pagas silenciosamente.

Fixe dependências no lock, use usuário sem privilégios e rode migrations explicitamente. Não inclua segredos na imagem. MLflow pode ser um serviço opcional local, sem bloquear a aplicação.

CI em push/pull request: Ruff, testes unitários, integração com PostgreSQL/pgvector e migrations em banco vazio, testes de segurança determinísticos, smoke de treino/serialização do classificador com fixture pequena e build da imagem. Use corpus e vetores fictícios pequenos; não chame LLM pago na CI padrão. Avaliação live é manual ou workflow separado com orçamento e segredos protegidos.

### Deploy

Escolha uma plataforma que suporte contêiner e PostgreSQL com pgvector. Confirme suporte à extensão, memória, conectividade de saída e custos antes da escolha final. AWS é uma opção, não requisito do MVP. Não contrate recursos pagos sem autorização já existente.

Entregue URL, configuração documentada, coleção pronta e um roteiro verificado: autenticar, perguntar sobre procedimento fictício, abrir fonte, testar pergunta sem resposta e registrar revisão. Documente backup/restauração do banco e teste uma restauração local.

Na release completa, o smoke test inclui classificar, consultar a versão do modelo e corrigir uma categoria, além do fluxo RAG.

Registre imagem, prompt, modelos, taxonomia e coleção implantados. Verifique compatibilidade de schema e retenção antes de rollback. Se houver bloqueio externo, registre a pendência e entregue o ambiente local sem alegar deploy concluído.

### Logs e observabilidade

Logs JSON com timestamp UTC, nível, request ID, rota normalizada, status HTTP, duração por etapa, coleção, modelos e código de erro sanitizado. Não inclua conteúdo completo, credenciais ou URLs sensíveis.

Mensure consultas, falhas, abstenções, feedback, latência, tokens e custo estimado; um script e consultas ao banco bastam no início. Mantenha IDs individuais em logs/traces, não como labels de métricas de alta cardinalidade.

Feedback vem de uma amostra selecionada: aprovação entre respostas revisadas não equivale à qualidade global. Mudança de distribuição e dashboards avançados são extensões.

## 12. Planejamento em oito sprints obrigatórias

As sprints 1–6 entregam o MVP RAG; as sprints 7–8 concluem o ciclo supervisionado de MLOps. As oito são obrigatórias para encerrar o projeto SupportAI. O primeiro deploy é um marco intermediário, não a conclusão do projeto.

Uma sprint é um marco de aprendizado, não promessa de prazo. Referência: 1 a 2 semanas, ajustadas à disponibilidade e ao orçamento. Comece localmente e não tente entregar toda a arquitetura em uma sessão.

### Sprint 1 — Domínio, corpus e avaliação

**Objetivo:** definir o que o assistente sabe e como será avaliado.

**Branch:** `feature/s1-knowledge-foundation`.

**Entregáveis:** estrutura mínima, dependências/lock, README inicial, plano, documentação dos dados, 20–40 documentos e cerca de 50 perguntas revisadas com partições e evidências de referência.

**Aceite:** fontes/licenças ou caráter fictício identificados; perguntas respondíveis e sem resposta presentes nas duas partições; paráfrases agrupadas; corpus e gabaritos separados; escopo e orçamento inicial registrados.

**Aprendizado:** diferença entre conhecimento do modelo, contexto recuperado e conjunto de avaliação.

### Sprint 2 — Ingestão e busca mensurável

**Objetivo:** recuperar evidências antes de gerar respostas.

**Branch:** `feature/s2-ingestion-retrieval`.

**Entregáveis:** banco/migrations, chunking, embeddings, coleção versionada/idempotente, busca textual e vetorial, avaliação de recuperação e MLflow inicial.

**Aceite:** repetir ingestão não duplica chunks; falha não publica coleção parcial; evidências são localizáveis; comparação Hit@k/Recall@k no desenvolvimento é reproduzível e informa contagens. Teste reservado permanece fechado.

**Aprendizado:** embeddings, distância vetorial, chunking, top-k e diferença entre busca textual e semântica.

### Sprint 3 — Geração e API RAG

**Objetivo:** responder com contexto e fontes verificáveis.

**Branch:** `feature/s3-rag-api`.

**Entregáveis:** integração com um LLM, prompt versionado, orçamento de contexto, resposta estruturada, abstenção, criação/consulta de histórico, autenticação e health checks.

**Aceite:** pergunta conhecida produz rascunho com fontes válidas; ausência de evidência gera abstenção; citação inexistente é rejeitada; timeout é erro técnico; falha não deixa sucesso parcialmente persistido.

**Aprendizado:** retrieval versus geração, grounding, janela de contexto e limite de citações.

### Sprint 4 — Revisão, segurança e avaliação de respostas

**Objetivo:** fechar o ciclo humano e identificar falhas de qualidade.

**Branch:** `feature/s4-review-evaluation`.

**Entregáveis:** aprovação/correção/rejeição, histórico e concorrência, rubrica humana, avaliação no desenvolvimento, testes de injection, tracing sanitizado e medição de tokens/custo.

**Aceite:** original preservado; revisor identificado no servidor; revisão desatualizada recebe `409`; metas do piloto documentadas; avaliação separa recuperação, sustentação e abstenção; logs/traces não capturam payloads sem autorização.

**Aprendizado:** avaliação de RAG, limites de LLM-as-a-judge, prompt injection e viés do feedback.

### Sprint 5 — Docker e CI

**Objetivo:** reproduzir o sistema em ambiente limpo com verificações automáticas.

**Branch:** `feature/s5-docker-ci`.

**Entregáveis:** Compose, build, health checks, CI offline com pgvector, instruções reais e backup/restauração local.

**Aceite:** ambiente sobe pelo README; ingestão é explícita; migrations e testes passam em banco novo; CI não usa chaves reais nem chamadas pagas; restauração local verificada.

**Aprendizado:** integração contínua, testes com fornecedores simulados e persistência do índice.

### Sprint 6 — Avaliação final e deploy do marco RAG

**Objetivo:** demonstrar a entrega e seus limites com evidências.

**Branch:** `feature/s6-demo-release`.

**Entregáveis:** configuração candidata congelada, avaliação no teste reservado, latência/custo em carga documentada, system card, deploy, manifesto de release e rollback.

**Aceite:** resultados com denominadores e versões; demo autenticado verificado com resposta, fonte, abstenção e revisão; orçamento/limites conhecidos; execução local reproduzível. Só marque deploy concluído após verificá-lo ou registre alteração de escopo para entrega local aceita pelo usuário.

**Aprendizado:** regressões de prompt/índice, release de sistema RAG e limites de um demo.

### Sprint 7 — Dados rotulados e treinamento supervisionado

**Objetivo:** aprender e executar o ciclo de treinamento e avaliação de um modelo próprio.

**Branch:** `feature/s7-supervised-training`.

**Entregáveis:** taxonomia e guia de anotação, dataset versionado, manifesto treino/validação/teste sem grupos compartilhados, baseline trivial, TF-IDF + regressão logística, seleção pequena de hiperparâmetros, runs MLflow e model card inicial.

**Aceite:** pipeline ajustado apenas no treino; comandos de treino/avaliação reproduzíveis; métricas por classe e comparação com baseline; artefato recarregável em processo separado; dataset, parâmetros, ambiente e resultados rastreáveis. Limiar de revisão escolhido na validação e teste final reservado para a candidata da release.

**Aprendizado:** rotulagem, vazamento, TF-IDF, regularização, macro-F1, desbalanceamento e registro de experimentos.

### Sprint 8 — Serving, retreino, monitoramento e release completa

**Objetivo:** operar e evoluir o modelo integrado ao produto com rastreabilidade.

**Branch:** `feature/s8-mlops-serving`.

**Entregáveis:** endpoints de classificação/revisão, modelo carregado na inicialização, artefato imutável e manifesto de release, testes/CI estendidos, métricas operacionais, segunda rodada de treino comparada, decisão de promoção e exercício de rollback. Atualize deploy, README, model card e system card.

**Aceite:** criar/consultar/corrigir classificação preserva original e versão; troca de modelo é explícita e rastreável; retreino usa dados curados e registra comparação, mesmo sem ganho; candidata final tem avaliação de teste identificada; rollback validado em local/staging; demo completo serve RAG e classificador sem retreinar durante requisições. Metas não atendidas permanecem registradas como limitação experimental.

**Aprendizado:** serving, promoção versus treinamento, monitoramento com feedback atrasado, retreino controlado e rollback de modelo.

### Extensões opcionais após a entrega completa

Escolha uma de cada vez, com hipótese e avaliação: busca híbrida, reranking, frontend simples, ingestão de PDF, embeddings locais ou Transformer para classificação. O classificador TF-IDF e seu ciclo de MLOps já são obrigatórios. Agentes e fine-tuning de LLM exigem um problema concreto que o fluxo simples não resolva.

## 13. Fluxo de trabalho e Git sem burocracia

### Primeira sessão de implementação

Inspecione o repositório e apresente um plano curto com escopo, estrutura proposta, sprints e decisões pendentes. Salve em `docs/PLAN.md`.

Se o usuário pediu apenas planejamento, entregue-o e aguarde. Se já autorizou a implementação, avance na primeira sprint; não solicite uma aprovação repetida. Não crie Git ou reorganize branches sem antes verificar o estado existente.

### Branches e commits

Use `main` estável e branches curtas de feature a partir dela. `develop` não é necessário para um projeto individual; se já houver um fluxo estabelecido no repositório, respeite-o.

Use Conventional Commits em inglês: `feat:`, `fix:`, `test:`, `docs:`, `refactor:` e `chore:`. Títulos claros, preferencialmente até 72 caracteres; corpo quando houver decisão relevante a explicar. Faça commits de unidades lógicas com verificações apropriadas, quando isso fizer parte do fluxo autorizado.

Evite commits gigantes, force push e reescrita de histórico compartilhado. Não faça merge, push ou publicação como consequência implícita de um pedido apenas de planejamento ou revisão. Quando integração estiver autorizada, use o fluxo do repositório sem exigir `--no-ff` por padrão.

### Didática por unidade lógica

Explique depois de uma mudança relevante ou conjunto coeso de commits:

```text
O que mudou: comportamento ou capacidade entregue.
Por que assim: decisão principal e seu trade-off.
Conceito: explicação breve com um exemplo.
Como verificar: comando real e resultado esperado.
Resultado: o que foi executado e eventuais limitações.
```

Não descreva cada arquivo ou cada commit mecanicamente. Aprofunde quando o conceito for novo ou o usuário pedir.

### Checkpoint de sprint

Apresente o que foi entregue, evidências de aceite, conceito aprendido e próximo passo. No modo didático padrão, aguarde “continuar” antes de iniciar a próxima sprint. Se o usuário já autorizou execução contínua, siga mantendo os checkpoints, sem pedir autorização repetida.

Não encerre uma sprint como concluída se algum critério obrigatório falhou; registre pendências e continue a correção dentro do escopo autorizado.

## 14. Documentação mínima mantida viva

- `README.md`: problema, arquitetura, instalação, comandos verificados, exemplos de API, resultados e limitações.
- `docs/PLAN.md`: sprints, status e mudanças de escopo.
- `docs/DATA.md`: corpus, licenças, ingestão, perguntas/evidências, partições, privacidade e retenção.
- `docs/SYSTEM_CARD.md`: uso pretendido, modelos, prompt/coleção, avaliação RAG, integração do classificador, custo, riscos, abstenção e revisão humana.
- `docs/MODEL_CARD.md`: tarefa de classificação, taxonomia, origem/versão dos dados, split, métricas por classe, limiar, limitações e versão promovida.
- `docs/PROJECT_LOG.md`: uma entrada curta por sprint com decisões, aprendizado, verificações e pendências.

O histórico do Git já lista commits; não mantenha uma cópia de cada mensagem em outro arquivo. CHANGELOG e ADRs são opcionais quando úteis. A system card descreve o produto RAG integrado; a model card descreve especificamente o classificador treinado. Evite duplicar resultados: use referências entre os arquivos.

Modelo enxuto de diário:

```markdown
## Sprint N — Título — AAAA-MM-DD
- Entrega e critérios de aceite:
- Decisões e justificativas:
- Conceito aprendido, com exemplo:
- Verificações executadas e resultados:
- Métricas, denominadores e versões do corpus/configuração:
- Custo observado e limitações:
- Pendências e próxima etapa:
```

## 15. Definição de pronto do projeto completo

O MVP RAG pode ser demonstrado antes, mas não encerra o projeto. Os itens abaixo incluem a trilha obrigatória de MLOps.

- [ ] Domínio delimitado, corpus autorizado e manifesto versionado.
- [ ] Perguntas/gabaritos revisados e separados do conteúdo indexado.
- [ ] Ingestão idempotente com coleção publicada apenas após validação.
- [ ] Busca textual e vetorial comparadas sob condições equivalentes.
- [ ] Geração com fontes resolvidas no servidor e abstenção explícita.
- [ ] Falhas técnicas diferenciadas de falta de evidência.
- [ ] Respostas originais e revisões autenticadas preservadas no PostgreSQL.
- [ ] Avaliação separada de recuperação, resposta e abstenção, com teste reservado.
- [ ] Corpus, prompts, modelos e configurações rastreáveis no MLflow/manifesto.
- [ ] Tokens, latência e custo medidos com suas limitações.
- [ ] Testes relevantes e CI passam sem chamadas pagas por padrão.
- [ ] Docker Compose funciona a partir de instruções verificadas.
- [ ] Demo verificado ou mudança de escopo para entrega local aceita e registrada.
- [ ] Logs/traces sanitizados, orçamento configurado e rollback documentado.
- [ ] Dataset de classificação tem rótulos revisados, taxonomia, manifesto e split sem vazamento.
- [ ] Baseline trivial e TF-IDF treinados, comparados e registrados no MLflow.
- [ ] Candidata final avaliada com métricas por classe e teste reservado documentado.
- [ ] API serve o pipeline aprovado e persiste categoria/score originais e versão.
- [ ] Revisão humana de categoria preserva histórico, autoria e concorrência.
- [ ] Artefato, checksum, ambiente, run ID e decisão de promoção constam no manifesto.
- [ ] Segunda rodada de treino comparada ao modelo vigente e decisão registrada.
- [ ] Monitoramento mínimo e exercício de rollback do classificador verificados.
- [ ] Deploy/smoke test cobre RAG e classificação, ou mudança de escopo de infraestrutura foi aceita.
- [ ] README, system card e model card permitem reproduzir e explicar a entrega.

As funcionalidades podem estar implementadas mesmo quando as metas de qualidade falham. Nesse caso, informe o estado experimental e o diagnóstico; não marque os critérios de qualidade como atendidos nem amplie o uso para respostas autônomas.

## 16. Escopo consolidado e histórico da mudança

- O núcleo principal continua sendo o assistente RAG com fontes, abstenção e revisão humana.
- A classificação de categoria deixa de ser extensão e passa a segundo marco obrigatório, com treinamento real de TF-IDF + regressão logística.
- Prioridade/equipe aprendidas, Transformer e fine-tuning de LLM continuam fora do escopo obrigatório.
- A avaliação RAG usa perguntas e evidências; a classificação usa rótulos, partições e métricas próprias. Nenhuma substitui a outra.
- O plano passa de seis para oito sprints para preservar a entrega incremental sem comprimir treinamento e operação em uma etapa simbólica.
- A release completa identifica corpus, coleção, embeddings, prompt, LLM, classificador, dados de treino e código.
- Mantidos: didática, arquitetura modular, FastAPI/PostgreSQL, Docker, MLflow, CI, deploy, revisão humana e pouca burocracia.

A conclusão exige evidências de arquitetura, treinamento e operação: explique como a resposta foi fundamentada, como o classificador foi treinado, como cada versão foi avaliada e como o sistema pode ser monitorado e revertido.
