# SupportAI — Domínio e dados

Atualizado em: 30/10/2026. Etapas: S1.1–S1.6 concluídas; S1.7 concluída por aceite explícito do usuário; S1.8 preparada, com revisão humana das novas anotações pendente.

O plano foi aprovado pelo usuário. O corpus atual tem 24 procedimentos fictícios em knowledge/, listados no manifesto 0.1.5-draft, todos com conteúdo aprovado por decisão explícita do usuário. A licença permanece pendente; nenhuma fonte está autorizada para ingestão. A S1.8 prepara 50 perguntas: 35 desenvolvimento e 15 teste, oito aprovadas no piloto e 42 novas pending. O piloto histórico está preservado em evaluation/rag_dev_pilot.json. Os novos arquivos e o manifesto de partições estão em evaluation/; nunca devem ser indexados. As validações de ingestão descritas abaixo são requisitos para a Sprint 2, ainda sem implementação.

## Produto e público

**NexoDesk** é o nome de trabalho de um produto SaaS fictício para organizar chamados de suporte em um espaço de trabalho. No cenário didático, uma organização possui usuários, administradores e atendentes. Essas funções do cenário ainda não definem o mecanismo de autorização da API.

O SupportAI auxilia o atendente dessa organização a consultar procedimentos de uso do NexoDesk e redigir respostas com evidências. Toda sugestão exige revisão humana; não haverá envio automático ao cliente.

O projeto começa com uma única organização, em português, e documentos Markdown inteiramente fictícios. Os procedimentos não descrevem políticas de um produto real. O nome é provisório e não pressupõe vínculo com serviços existentes.

## Cobertura da base

| Área | Situações previstas | Limite |
| --- | --- | --- |
| Conta e acesso | Recuperação de acesso, perda de autenticador, convites e permissões de uso. | Explicar procedimentos documentados; não validar identidade, desbloquear contas ou contornar autenticação. |
| Uso básico e falhas conhecidas | Criar e acompanhar chamados, usar filtros e consultar soluções documentadas para problemas da interface. | Não diagnosticar ambientes externos ou afirmar o estado de serviços em tempo real. |
| Cobrança | Consultar faturas e explicar condições fictícias de plano e cobrança quando documentadas. | Não processar pagamentos nem prometer exceções e reembolsos ausentes da base. |
| Solicitações de serviço | Orientar pedidos de alteração de plano e configurações previstas. | Não executar alterações nem conceder permissões. |

Essas áreas organizam a cobertura documental. Não fixam os rótulos do classificador: a taxonomia de 4–6 categorias será definida com os chamados e exemplos de fronteira na Sprint 7. A categoria sugerida não restringirá a busca RAG.

## Exemplos para conferir o escopo

Os exemplos abaixo são casos de uso ilustrativos. Ainda não são perguntas anotadas de avaliação e não possuem respostas de referência.

| Pergunta | Decisão de escopo | Evidência necessária ou limitação |
| --- | --- | --- |
| “Troquei de celular e perdi acesso ao autenticador do NexoDesk. Como solicito recuperação?” | Coberta: conta e acesso. | Será necessário um procedimento fictício de recuperação com condições e encaminhamento. Não inventar passos antes de ele existir. |
| “Onde consulto a fatura do meu plano no NexoDesk?” | Coberta: consulta de cobrança. | Será necessário um procedimento com o caminho e quem pode acessá-lo. |
| “Como acompanho o andamento de um chamado no NexoDesk?” | Coberta: uso básico. | Será necessário um procedimento de acompanhamento e significado dos estados. |
| “Como recupero a senha da minha conta bancária?” | Fora do domínio: outro produto. | Nenhum procedimento do NexoDesk sustenta essa orientação. |
| “Meu contrato dá direito a uma exceção de reembolso não descrita na base?” | Fora da cobertura: decisão contratual particular. | Informar a limitação e encaminhar à revisão humana, sem inferir direitos ou condições. |
| “Vocês estão com uma indisponibilidade agora?” | Fora da capacidade: estado em tempo real. | A base estática não confirma incidentes atuais; não presumir disponibilidade nem indisponibilidade. |

**Estar dentro do domínio não garante que uma pergunta seja respondível.** Mesmo a pergunta sobre fatura exige abstenção se o procedimento estiver ausente, conflitante ou não cobrir a situação. Se faltar contexto, pedir esclarecimento em vez de escolher um procedimento por suposição.

Falta de evidência e falha técnica serão tratadas separadamente na API futura: ausência de procedimento permite `insufficient_evidence`; timeout do fornecedor é erro técnico. Citações precisam sustentar as afirmações, além de apontar para IDs existentes.

## Fontes e separação dos dados

- Corpus atual: 24 documentos fictícios (24 com conteúdo aprovado pelo usuário); meta de expansão atingida, dentro da faixa de 20–40. Piloto aprovado na S1.6; expansão em lotes na S1.7.
- Origem do piloto: redação assistida por LLM para este projeto em 30/10/2026, sem fontes externas ou dados de clientes. Autoria declarada como SupportAI project; conteúdo do piloto aprovado pelo usuário na S1.6. Nenhum texto gerado está automaticamente aprovado.
- Avaliação atual: 8 perguntas do piloto em desenvolvimento, anotadas com assistência de LLM e aprovadas pelo usuário na S1.6. Meta final: cerca de 50 revisadas, aproximadamente 35 de desenvolvimento e 15 de teste, com grupos de paráfrases preservados. As 8 perguntas do piloto permanecerão em desenvolvimento; nenhum teste reservado foi criado ou usado.
- `knowledge/` conterá procedimentos; `evaluation/`, perguntas e gabaritos. Somente documentos autorizados por manifesto poderão ser indexados. Este documento de escopo também não é fonte de procedimentos para ingestão.
- Chamados supervisionados terão conjunto próprio na Sprint 7; perguntas RAG não substituem exemplos rotulados de classificação.
- Não coletar dados reais de clientes, credenciais ou documentos privados nesta etapa. Nenhum conteúdo será enviado a fornecedores externos na Sprint 1.
- Licenças de código e materiais autorais permanecem pendentes. Não atribuir licença por suposição nem tratar ficção como permissão automática de redistribuição.
- Metadados, atualização e retenção/exclusão estão definidos abaixo. Não há dados operacionais armazenados neste momento.

## Contrato dos documentos — versão 1

Escolha inicial: manter os metadados no manifesto JSON e o procedimento em Markdown UTF-8. Isso evita adicionar um parser de front matter e duplicar versões/licenças em dois lugares. O manifesto é a autoridade dos metadados; o título e as seções declarados devem corresponder ao Markdown. JSON é lido pela biblioteca padrão do Python.

| Campo de cada entrada em documents | Regra |
| --- | --- |
| `document_id` | String estável, única no snapshot, como `nd-access-001`. Não muda quando o conteúdo é revisado e não é reutilizada para outro procedimento. |
| `title` | Título não vazio, igual ao único título principal `#` do arquivo. |
| `version` | Versão editorial no formato `MAJOR.MINOR.PATCH`, como `1.0.0`. Qualquer alteração nos bytes exige nova versão; mudança no procedimento ou suas condições exige revisão humana novamente. |
| `path` | Caminho relativo ao repositório, dentro de `knowledge/`, para um único arquivo `.md`. Nenhum glob, diretório ou URL. |
| `sha256` | Hash dos bytes do arquivo final: 64 caracteres hexadecimais minúsculos. Pode ser null apenas em um rascunho; nunca em uma entrada autorizada. Não fica dentro do arquivo que identifica. |
| `language` | `pt-BR` nesta versão do corpus. |
| `source` | Objeto com `kind`, `reference` e `obtained_on`. Nesta sprint, kind é `fictional`; reference é o caminho local de origem, sem download; obtained_on é a data real de incorporação, `YYYY-MM-DD`. |
| `author` | Autoria declarada do material: pessoa ou identificação do projeto, sem atribuir a uma pessoa uma revisão que ela não fez. |
| `creation_method` | `human_authored` ou `llm_assisted`. Texto redigido pelo agente é llm_assisted, mesmo após correções humanas. |
| `license` | Objeto com `status`, `identifier` e `reference`. Status `pending` ou `approved`; identifier/reference podem ser null enquanto pendentes. Aprovação exige identificar os termos e onde estão registrados. |
| `updated_on` | Data real de atualização do documento, `YYYY-MM-DD`. Não inventar datas passadas de um produto fictício. |
| `review` | Objeto com `status`, `reviewed_by` e `reviewed_at`. Status `pending` ou `approved`; autoria da revisão humana e timestamp UTC com timezone são obrigatórios para aprovação. Enquanto pendente, ambos são null. |
| `sections` | Lista não vazia de objetos com `section_id` e `title`, correspondentes às seções `##` do Markdown. IDs únicos dentro do documento e estáveis enquanto a seção conservar sua identidade. |

Formato das seções: `## recovery-request — Solicitar recuperação`. O prefixo `recovery-request` é o section_id; o restante é o título. Usar IDs ASCII em minúsculas, com números e hífens; todo conteúdo de procedimento deve estar sob uma seção `##`. No piloto, não usar subtítulos mais profundos: listas e parágrafos bastam para preservar os passos.

Trechos futuros referenciarão documento, versão, section_id, posição no texto e configuração de chunking. As perguntas de avaliação apontarão para passagens no documento original, de modo que mudar o chunking não destrua o gabarito. A convenção de offsets será `[start, end)` por caracteres Unicode do texto UTF-8 decodificado, sem normalização de acentos ou quebras de linha; registrar a passagem literal para conferência.

### Modelo de entrada, sem autorização de ingestão

O exemplo é um molde de preenchimento, com datas e checksum omitidos. O arquivo nd-access-001 agora existe no piloto; sua entrada real e seu checksum estão no manifesto. Não usar este molde no lugar da entrada real nem copiá-lo para um manifesto aprovado sem revisão.

```json
{
  "document_id": "nd-access-001",
  "title": "Recuperação de acesso após perda do autenticador",
  "version": "1.0.0",
  "path": "knowledge/nd-access-001.md",
  "sha256": null,
  "language": "pt-BR",
  "source": {
    "kind": "fictional",
    "reference": "knowledge/nd-access-001.md",
    "obtained_on": null
  },
  "author": "SupportAI project",
  "creation_method": "llm_assisted",
  "license": {
    "status": "pending",
    "identifier": null,
    "reference": null
  },
  "updated_on": null,
  "review": {
    "status": "pending",
    "reviewed_by": null,
    "reviewed_at": null
  },
  "sections": [
    {"section_id": "recovery-request", "title": "Solicitar recuperação"}
  ]
}
```

Datas null são permitidas somente no molde documental acima; ao incluir uma entrada real, preencher source.obtained_on e updated_on. Licença e revisão podem continuar pendentes em um manifesto draft. Ter checksum válido comprova integridade, não correção do procedimento, licença ou aprovação humana.

## Manifesto autorizado — versão 1

Arquivo: `configs/corpus_manifest.json`. Ele lista explicitamente os arquivos candidatos; não se deve varrer o repositório nem inferir autorização pela presença em `knowledge/`.

| Campo | Regra |
| --- | --- |
| `schema_version` | Inteiro `1`; versão do contrato. Rejeitar versões desconhecidas. |
| `corpus_version` | Identificador do snapshot, como `0.1.0-draft` em elaboração e `0.1.0` após aprovação. Não confundir com versão individual de documento ou de coleção vetorial. |
| `status` | `draft` ou `approved`. Draft permite preparar material, mas não autoriza ingestão, embeddings ou publicação. Approved exige cumprir todas as regras abaixo. |
| `language` | `pt-BR`. |
| `document_count` | Inteiro igual ao tamanho de documents. Snapshot aprovado tem entre 1 e 40 entradas; o piloto pode ter 3, mas o aceite final da Sprint 1 continua exigindo 20–40. |
| `documents` | Lista de entradas com o contrato acima. Snapshot vazio é permitido apenas como draft. |

**Estado atual:** 24 documentos em 0.1.5-draft, status draft, 24 revisões aprovadas, todas as licenças pendentes, nenhuma fonte autorizada para ingestão. O snapshot vazio anterior era 0.1.0-draft, preservado no histórico do Git. Não é uma coleção pronta e não satisfaz readiness da API. A revisão do corpus é distinta dos estados building/ready da coleção futura.

### Requisitos de validação para a ingestão futura

1. Ler apenas o manifesto selecionado pelo comando administrativo local. Limite inicial: 2 MiB de JSON; rejeitar chaves duplicadas, campos desconhecidos, tipos inválidos e contagens divergentes em vez de adivinhar o formato.
2. Exigir status approved, pelo menos um documento, licença aprovada e revisão humana registrada em todas as entradas. Não pular entradas inválidas silenciosamente; não iniciar chamadas externas antes da validação local completa.
3. Aceitar caminhos POSIX sob `knowledge/`, com extensão `.md` e componentes em ASCII minúsculo com letras, números, hífen ou sublinhado. Rejeitar caminhos absolutos, `..`, `.`, componentes vazios, barras invertidas, URLs e globs. Rejeitar symlinks, inclusive na pasta raiz e em componentes intermediários; verificar o caminho real dentro da raiz permitida, sem seguir atalhos para fora dela.
4. Exigir arquivos regulares, UTF-8 válidos, não vazios e até 128 KiB cada. Conferir SHA-256 dos bytes antes de normalizar ou dividir o texto. Rejeitar IDs, caminhos e hashes de conteúdo duplicados; revisar duplicatas semânticas manualmente.
5. Conferir título, seções e metadados; rejeitar conflitos e documentos sem passos/condições suficientes na revisão humana. Não usar o agente como substituto do revisor humano.
6. Excluir sempre `evaluation/`, `docs/`, `data/`, saídas de testes e feedback não curado. Nem um caminho listado no manifesto pode escapar dessas restrições.
7. Registrar versão e SHA-256 do manifesto final em cada ingestão/experimento. Esse hash é calculado externamente, sem autorreferência no próprio JSON. Aprovação não significa executar ingestão automaticamente.

O limite por arquivo é uma proteção de entrada; não define tamanho de chunk ou orçamento de tokens. Esses limites serão medidos e configurados separadamente na Sprint 2.

### Como conferir um checksum do piloto

Executar na raiz do repositório:

```bash
sha256sum knowledge/nd-access-001.md
```

O resultado deve coincidir com sha256 na entrada nd-access-001 do manifesto. Na S1.4, os três hashes foram calculados dos bytes finais e conferidos novamente de forma independente. Não há ingestão implementada.

## Piloto documental — S1.4

| Documento e versão | Escopo e evidências disponíveis | Ausências intencionais |
| --- | --- | --- |
| nd-access-001, 1.0.0 | Solicitação de recuperação após perda de 2FA pela tela de entrada, dados mínimos, protocolo e exceções. | Critérios internos de identidade, prazo de conclusão e acompanhamento sem autenticação. |
| nd-billing-001, 1.0.0 | Consulta e download de fatura por administrador do espaço; ausência de fatura, permissões e divergência. | Regras de reembolso, descontos, pagamento, prazo de análise e dados financeiros atuais. |
| nd-tickets-001, 1.0.0 | Consulta dos próprios chamados após login, interpretação de quatro situações e resposta ao histórico. | Busca interna por atendentes, consulta pública por protocolo, prazos e reabertura. |

Escolhas fictícias reversíveis: conta com autenticador; espaço de trabalho selecionado; faturas restritas ao perfil Administrador do espaço de trabalho; lista Meus chamados limitada à conta solicitante. Essas são regras do cenário documental, não permissões já implementadas na API do SupportAI.

Revisão de coerência pelo agente: recuperação começa sem login, mas acompanhamento exige autenticação; não há consulta pública por protocolo. Um protocolo confirma registro, não aprovação. Faturas não aparecem para qualquer atendente; situação Concluído não implica concessão do pedido. O chamado de divergência de cobrança usa o mesmo menu Ajuda do documento de acompanhamento. Nenhum documento promete prazo ou reembolso.

Os três rascunhos têm passos ordenados, condições e exceções, com 11 seções estáveis no total. Não usam referências a procedimentos inexistentes; os IDs citados entre documentos pertencem ao próprio piloto. Isso é uma revisão editorial do agente, sem substituir a revisão humana da S1.6 ou uma avaliação do sistema.

As lacunas foram usadas para preparar perguntas sem resposta na S1.5. Elas não autorizam completar regras com conhecimento externo. Não há métricas de recuperação, embeddings ou chamadas a modelos de geração no fluxo do produto.

## Perguntas de desenvolvimento — S1.5

Arquivo: `evaluation/rag_dev_pilot.json`. Formato JSON versão 1, evaluation_version 0.1.1-draft e status draft (licença pendente), com as oito revisões de conteúdo aprovadas. O envelope registra idioma, data, split, corpus_version e SHA-256 do manifesto de origem. Esse vínculo identifica o snapshot exato; alterar o corpus exige revisar as anotações e publicar outra versão da avaliação, sem atualizar hashes silenciosamente.

São 8 perguntas em 7 grupos: 6 respondíveis e 2 sem resposta, todas com split development. Perguntas e gabaritos foram redigidos com assistência de LLM a partir dos três documentos fictícios, sem fontes externas. As oito anotações foram aprovadas por decisão explícita do usuário na S1.6; não há segundo anotador independente. O arquivo não é um dataset de treinamento do classificador.

| ID | Caso avaliado | Grupo | Respondível |
| --- | --- | --- | --- |
| rag-dev-001 | Solicitação de recuperação após perda do autenticador, com e-mail disponível. | access-recovery-request | Sim |
| rag-dev-002 | Paráfrase da anterior com abreviações cel. e 2FA. | access-recovery-request | Sim |
| rag-dev-003 | Consultar e baixar fatura de um mês. | invoice-lookup | Sim |
| rag-dev-004 | Menu Cobrança ausente e limite de permissões. | invoice-access-permission | Sim |
| rag-dev-005 | Acompanhar um chamado da própria conta. | own-ticket-tracking | Sim |
| rag-dev-006 | Ambiguidade sobre Concluído e aprovação do pedido. | ticket-completion-meaning | Sim, para explicar o significado; não para afirmar a decisão do caso real. |
| rag-dev-007 | Prazo de reembolso não documentado. | refund-policy-unavailable | Não |
| rag-dev-008 | Problema não identificado na pergunta; precisa de contexto. | missing-problem-context | Não |

### Contrato das anotações

- `question_id`, `question`, `question_types`, `group_id` e `split` identificam texto, cobertura e agrupamento. Os tipos são marcadores de curadoria e podem se sobrepor; não são categorias previstas pelo classificador.
- `answerable=true` exige reference_answer não vazia e ao menos uma evidence_span. O gabarito resume apenas afirmações sustentadas pelas passagens, sem exigir correspondência literal com uma futura resposta do sistema. `relevant_document_ids` corresponde aos documentos das evidências.
- Cada evidence_span registra document_id, document_version, document_sha256, section_id, start, end e text. Os offsets são relativos ao documento Markdown completo decodificado como UTF-8, em caracteres Unicode, intervalo `[start, end)`, incluindo a sintaxe Markdown original e sem normalização. Conferir `document_text[start:end] == text`; os limites devem ficar dentro da seção declarada. Não são offsets de bytes ou tokens.
- `answerable=false` tem reference_answer null, relevant_document_ids e evidence_spans vazios, abstention_reason e expected_abstention preenchidos. Mesmo quando o documento declara que uma regra não está definida, não fabricar uma evidência positiva ou uma resposta para a regra ausente. `clarification_question` contém a pergunta de esclarecimento quando útil, como em rag-dev-008.
- Em casos respondíveis, abstention_reason, expected_abstention e clarification_question são null. Em todos os casos, annotation_source é llm_assisted e human_review começa pending, com reviewed_by e reviewed_at nulos. Revisão pelo agente não vira revisão humana.

### Separação e uso futuro

No snapshot do piloto, o manifesto de ingestão listava somente os três Markdown de knowledge/; no atual, são 24. Nenhum arquivo de avaliação é fonte de ingestão. Nunca enviar reference_answer, evidence_spans ou expected_abstention como entrada da busca ou contexto de geração; a busca recebe a pergunta, e o contexto futuro contém somente os trechos recuperados. Os gabaritos ficam do lado do avaliador.

rag-dev-001 e rag-dev-002 permanecem no mesmo grupo e partição. Como as oito perguntas já foram usadas neste piloto e serão discutidas na revisão, elas não podem ser promovidas a teste reservado. A proposta de divisão da S1.8 está descrita ao final; o piloto histórico não possui partição de teste.

Para recuperação, o denominador inicial será de 6 perguntas respondíveis; perguntas sem resposta não entram no Recall@k. As unidades de evidência são as passagens anotadas, a mapear para chunks na Sprint 2. A regra de correspondência e o tratamento de sobreposições ainda precisam ser documentados antes de calcular métricas; não contar cada paráfrase como grupo independente nem tratar 6 casos como evidência estatística ampla.

A rubrica humana deve conferir cobertura e sustentação da resposta de referência, pertinência das passagens e motivo da abstenção. As oito perguntas ainda não foram executadas contra um sistema; contagens de anotações não são resultados de desempenho. Prompt injection terá suíte específica nas etapas de segurança.

## Atualização, revisão e licença

- Origem autorizada inicialmente: somente documentos fictícios elaborados para o projeto, em português, com autoria e assistência de LLM declaradas. Documentos públicos de terceiros exigirão curadoria e compatibilidade de licença antes de inclusão; não haverá scraping aberto.
- Revisar condições, passos, exceções e coerência entre procedimentos antes de aprovar cada versão. Não preencher reviewed_by ou reviewed_at automaticamente como se o usuário tivesse revisado.
- Enquanto a licença de materiais autorais estiver pendente, os documentos podem ser preparados localmente em draft, mas não promovidos a approved nem publicados. Não escolher uma licença para o usuário por suposição. A decisão sobre licença fica explícita no checkpoint do piloto e precede qualquer promoção.
- Snapshot aprovado é imutável. Alterar documento, metadados ou seleção de fontes gera outra corpus_version e outro hash de manifesto; conteúdo alterado também exige nova versão e checksum do documento. Recalcular checksums depois de finalizar o arquivo, não antes.
- Documento removido deixa de aparecer no próximo snapshot. A Sprint 2 deverá criar outra coleção, validar e só depois trocar a referência ativa; falha parcial mantém a anterior. Não reaproveitar chunks obsoletos na coleção nova.
- Mudanças de regras invalidam as anotações afetadas: revisar passagens e respostas esperadas. Se uma decisão for influenciada pelo teste reservado, registrar a exposição; não reaproveitá-lo como se continuasse intocado.
- Feedback de atendentes não altera corpus, gabaritos ou treino automaticamente. Curadoria e anonimização vêm antes da incorporação; deduplicar contra grupos reservados.

## Privacidade, retenção e exclusão

Política inicial de demonstração, a implementar nas sprints responsáveis. Os prazos são máximos de retenção e escolhas reversíveis do projeto; não representam garantias de exclusão já disponíveis.

| Material | Retenção proposta | Exclusão e restrições |
| --- | --- | --- |
| Corpus e avaliação fictícios revisados, manifestos e metadados seguros | Versionados no Git enquanto o projeto for mantido. | Somente conteúdo sem dados pessoais e com termos de uso registrados. Licença pendente impede publicação. |
| Coleções inativas e snapshots usados como fontes históricas | Até 30 dias após desativação. | Manter ao menos a versão anterior dentro dessa janela para rollback; após expiração, informar fonte histórica indisponível em vez de apontar para conteúdo novo. |
| Perguntas, respostas, classificações e revisões operacionais | Até 30 dias após criação da consulta/classificação. Revisão não reinicia a janela. | Remover conteúdo e relações dependentes; não manter originais pessoais indefinidamente sob justificativa de auditoria. Expor indisponibilidade após expiração. |
| Logs e traces sanitizados | Até 14 dias após emissão. | Sem perguntas, respostas, textos de chamados ou contextos completos por padrão; sem credenciais. Usar IDs e códigos de erro sanitizados. |
| Backups locais de dados operacionais | Até 30 dias após criação do backup. | Acesso restrito, fora do Git; aplicar exclusões pendentes antes de disponibilizar qualquer restauração. Não alegar remoção imediata de cópias offline. |
| Chamados brutos privados | Não coletados no escopo inicial. | Não copiar para o repositório ou fornecedores. Reavaliar origem, acesso e retenção antes de usar outra fonte na Sprint 7. |
| Dados supervisionados fictícios curados e artefatos aprovados | Manifestos seguros no Git; artefatos fora do Git. Política específica antes do treino na Sprint 7. | Verificar ausência de dados pessoais e origem controlada; checksum antes de carregar o modelo. Não reter material privado por esta regra de dados fictícios. |

Se surgir conteúdo pessoal ou segredo: interromper o processamento e envio externo, retirar da lista autorizada e da coleção ativa, identificar cópias/derivados e registrar a exclusão sem repetir o conteúdo no log. Revogar credenciais expostas. A exclusão de traces, chunks, backups e eventual histórico publicado exige tratamento próprio; um novo commit que remove um arquivo não apaga suas versões anteriores. Não publicar material suspeito.

Antes do demo, implementar expiração e exclusão no banco, armazenamento e tracking; verificar limpeza e restauração sem reintroduzir conteúdo excluído. Manter registro mínimo protegido dos IDs e prazo de expurgo necessários para aplicar exclusões em backups, sem guardar o texto removido. Exclusão de dados pessoais prevalece sobre rollback e preservação de fontes históricas.

O uso inicial permanece sem dados reais e sem envio a fornecedores na Sprint 1. Nas sprints de integração, revisar política do fornecedor e orçamento antes de enviar conteúdo. Os dois conjuntos de avaliação e os dados de treino continuam separados; publicar contagens e limitações de dados sintéticos.

## Orçamento e limitações

Orçamento inicial da Sprint 1: **R$ 0 em APIs e hospedagem**, com trabalho local e revisão manual. O orçamento de fornecedores será definido antes de chamadas pagas nas etapas seguintes.

O corpus fictício permite avaliar se o sistema segue procedimentos controlados; seus resultados não demonstram desempenho em chamados reais. Não haverá execução de ferramentas pelo LLM, fine-tuning de LLM ou treinamento de embeddings no MVP.

## Verificação da S1.1

Na conclusão da S1.1, os três casos cobertos correspondiam às áreas declaradas e os três casos fora da cobertura tinham motivos explícitos. Naquele momento, o corpus, o conjunto de avaliação e a aplicação ainda não existiam. O piloto documental foi criado depois, na S1.4.

## Verificação da S1.3

Contrato, molde e manifesto vazio definidos; validação de JSON, contagem e estado draft conferida. A identificação do documento combina ID estável, versão, SHA-256 e section_id. Licença e revisão pendentes bloqueiam autorização; não foram inventadas aprovações ou hashes de documentos inexistentes.

## Verificação da S1.4

Três procedimentos fictícios redigidos; manifesto com contagem 3, caminhos reais, IDs e hashes únicos, metadados, datas reais de criação e 11 seções correspondentes ao Markdown. SHA-256 conferido sobre os bytes finais de cada arquivo; nenhum hash inserido no próprio documento. Licença pending, revisão pending e campos de revisor/timestamp nulos em todas as entradas.

## Verificação da S1.5

Conferidos JSON, IDs, contagens (6 respondíveis, 2 sem resposta, 7 grupos), split único development, checksums do snapshot e documentos, correspondência literal das passagens e seus limites de seção. Os exemplos sem resposta não têm evidências inventadas; todas as revisões humanas permanecem pending. Corpus e manifesto de ingestão foram mantidos sem alteração.

## S1.6 — Histórico de preparação e decisão

O roteiro docs/PILOT_REVIEW.md apresenta os três documentos por links, as oito perguntas com gabaritos/passagens e decisões pendentes. Ele identifica por checksum os snapshots apresentados ao revisor e destaca a distinção entre explicar o estado Concluído e afirmar uma decisão de um chamado real.

Na conferência editorial do agente, não foi identificada uma contradição que exigisse alteração antes da revisão humana. Foram preservados corpus 0.1.1-draft, avaliação 0.1.0-draft, offsets, grupos e checksums. Nenhum campo de revisão humana ou licença foi aprovado automaticamente.

Para encerrar a etapa, o usuário deve avaliar o material, aprovar ou solicitar correções por ID e informar a identificação a registrar como revisor. Aprovação de conteúdo é distinta de permissão de publicação: a licença pode permanecer pendente por decisão explícita, mantendo o manifesto em draft. Se houver apenas uma pessoa revisando, declarar a ausência de segundo anotador independente; não inferir revisão de compreensão do usuário por resultados automáticos.

**Decisão vigente:** usuário aprovou os três documentos e oito anotações em 30/10/2026 e autorizou a S1.7. Registro project-user, timestamp 2026-10-30T15:53:10Z. Licença mantida pendente. A descrição acima preserva o histórico da preparação; não há segundo revisor humano independente. Revisão de conteúdo não autoriza ingestão ou publicação. Os avisos de rascunho nos arquivos originais permanecem como histórico de preparação; o manifesto registra a revisão vigente.

## S1.7 — Expansão, lote 1

Adicionados nd-tickets-002 (abrir chamado), nd-tickets-003 (responder a pedido de informação) e nd-tickets-004 (filtrar os próprios chamados), versão 1.0.0, criação assistida por LLM em 30/10/2026. Revisão humana e licença pendentes. O roteiro completo está em docs/S1_7_BATCH_REVIEW.md. O corpus tem 6 documentos e 23 seções, rumo à meta de 24 documentos; a etapa não está concluída.

A avaliação 0.1.1-draft continua vinculada ao corpus 0.1.2-draft de três documentos do commit 7acb503. Não atualizar seu hash para o corpus expandido sem revisar as anotações: a adição de fontes pode tornar uma pergunta antes sem resposta respondível. Neste lote, a conferência editorial preservou as duas abstenções, sem executar modelo.

## S1.7 — Expansão completa e aceite pendente

A pedido do usuário, os lotes restantes foram redigidos e conferidos editorialmente nesta sessão. Corpus 0.1.4-draft: 24 documentos e 95 seções; acesso 6, chamados 4, cobrança 4, serviço 5 e problemas de interface 5. O índice de todos os lotes, regras e verificações está em [S1_7_REVIEW.md](S1_7_REVIEW.md). A seção anterior do lote 1 é histórica.

Os 21 documentos novos permanecem com revisão humana pending e os três do piloto continuam approved. Todas as licenças permanecem pending; nenhuma fonte autorizada para ingestão. Redação e conferência da expansão concluídas; aceite integral da S1.7 depende da decisão humana sobre o conteúdo novo. A avaliação permanece vinculada ao snapshot original; revalidar seu vínculo na S1.8 antes de avaliar o novo corpus.

## Aceite explícito da S1.7 — 30/10/2026

A aprovação dos 21 documentos de expansão foi solicitada explicitamente pelo usuário após o resultado das verificações locais. Registro `project-user`, `2026-10-30T16:09:45Z`. Conteúdo dos 24 documentos aprovado; snapshot `0.1.5-draft`, licença pendente e nenhuma ingestão/publicação. O Markdown não mudou; avisos de preparação nos arquivos são históricos, e a revisão vigente consta no manifesto. Não houve segundo revisor humano independente. A S1.8 foi autorizada.

## S1.8 — Avaliação expandida para revisão

Versão `0.2.0-draft`, ligada explicitamente ao corpus `0.1.5-draft` e SHA-256 do manifesto. `evaluation/rag_development.json` contém 35 perguntas, 28 respondíveis e 7 sem resposta, em 22 grupos. `evaluation/rag_test.json` contém 15 perguntas, 11 respondíveis e 4 sem resposta, em 11 grupos. `evaluation/rag_split_manifest.json` registra checksums dos dois arquivos, IDs, grupos, contagens e vínculo com o corpus. Todos os 24 documentos aparecem como evidência no conjunto completo; isso não garante cobertura de todas as suas regras.

As oito anotações originais foram copiadas sem alteração para desenvolvimento; seu arquivo e snapshot de origem permanecem preservados. Sua respondibilidade foi reavaliada editorialmente diante dos 24 documentos: seis continuam sustentadas e as duas abstenções continuam adequadas. Não se atribuiu nova revisão humana a essa conferência. As 42 novas anotações são llm_assisted e human_review pending; aceite dos documentos não aprova perguntas ainda não apresentadas. A revisão está organizada em cinco lotes (9/9/9/8/7) em [S1_8_REVIEW.md](S1_8_REVIEW.md).

A divisão é manual por grupos de intenção, definidos antes de atribuir partições, sem sorteio/seed. Paráfrases e variações do mesmo procedimento ficam juntas. Não há grupos compartilhados; compartilhar documentos é permitido na avaliação sobre uma base fixa. O teste está proposto, não congelado (`test_frozen=false`). Foi lido para anotação/conferência, sem executar modelo ou ajustar configurações por seus resultados. Congelar apenas após revisão humana; não usar suas perguntas para escolher retrieval, prompts ou limiares. Qualquer uso que influencie decisões deve ser registrado e exigir revisão da reserva.

Passagens usam caracteres Unicode `[start, end)` e seções/documentos versionados. Foram conferidos checksums, contagens, unicidade, grupos, passagens literais e limites de seção, contratos de abstenção/revisão, links e preservação dos arquivos de origem. Os denominadores propostos de recuperação são 28 e 11 perguntas respondíveis, respectivamente; não há métricas calculadas. A correspondência de passagens com chunks será definida na Sprint 2. Corpus fictício, ausência de segundo anotador e amostra pequena limitam a interpretação futura. S1.8 aguarda revisão humana e congelamento do teste; S1.9 não iniciada.
