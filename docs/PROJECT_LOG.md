# SupportAI — Diário de aprendizado

## Sprint 1 — S1.1: domínio e escopo — 2026-10-28

- Entrega: domínio NexoDesk documentado em DATA.md; público, cobertura, exclusões, três perguntas cobertas e três fora da cobertura; README e plano atualizados.
- Decisão: manter o cenário fictício aprovado no plano, uma organização e revisão humana. Isso permite elaborar procedimentos controlados sem usar dados reais de clientes.
- Conceito: domínio delimita os assuntos; evidência determina se uma pergunta pode ser respondida. Consultar uma fatura está no domínio, mas exige um procedimento documentado para sustentar a resposta.
- Verificações: leitura cruzada dos seis exemplos e limites em DATA.md; conferência de coerência com README e PLAN.md; verificação de espaços e conflitos com git diff --check.
- Métricas: não aplicáveis nesta etapa documental; nenhum corpus, avaliação ou modelo foi produzido.
- Custo: nenhuma chamada externa paga ou contratação; orçamento da Sprint 1 fixado em R$ 0 para APIs e hospedagem.
- Pendências: ambiente mínimo (S1.2), metadados/políticas de dados (S1.3), piloto e revisão humana. Licenças permanecem sem definição. Sprint 1 em andamento.

## Sprint 1 — S1.2: ambiente mínimo reproduzível — 2026-10-28

- Entrega: .python-version com CPython 3.12.16, pyproject.toml, único uv.lock, exclusões no .gitignore e comandos de instalação/verificação no README. uv 0.13.6 isolado em .venv-tools; ambiente do projeto em .venv; Python gerenciado e cache em .cache. Python do sistema preservado.
- Decisão: série Python 3.12 em suporte de segurança, patch específico fixado; uv gerencia o ambiente e o lock. Apenas pytest e Ruff como dependências diretas de desenvolvimento. Adiar bibliotecas de API, banco e ML até sua utilização; package=false evita criar um pacote vazio. O suporte da série não implica que o patch nunca precise de atualização.
- Conceito: pyproject.toml declara faixas de dependências; uv.lock registra a resolução exata e hashes; .venv materializa a instalação local. Exemplo: a faixa de pytest aceita versões 8 e 9, mas este lock instala especificamente 9.2.0.
- Verificações executadas: TOML lido com tomllib; uv lock --check --offline; comandos de versão de Python, pytest e Ruff; uv pip check. Resultado: Python 3.12.16, pytest 9.2.0, Ruff 0.17.3, seis pacotes instalados compatíveis. O lock também contempla colorama para Windows, não instalado no Linux.
- Reprodução: uv sync --locked --offline em um segundo ambiente vazio temporário, usando o cache local; comparação de Python e inventário de pacotes idêntica ao ambiente principal; compatibilidade verificada novamente; ambiente temporário removido ao fim. SHA-256 de uv.lock permaneceu cd5899d4204845dbd751ddce93956ba3e7b54ac4d7a9fce03d4f05ab10341038.
- Verificação de arquivos locais: ambientes, cache e .env ignorados pelo Git; .env.example permanece permitido. Não foram criadas fixtures, módulos vazios ou testes artificiais.
- Limites: verificação realizada em Linux x86_64 nesta máquina. A primeira instalação usa rede; a recriação offline depende do Python e pacotes já baixados. Não há suíte de testes ou aplicação; pytest --version não representa testes aprovados. A compatibilidade com as futuras bibliotecas de RAG e ML ainda não foi avaliada.
- Acesso: a primeira revisão automática de liberação de rede expirou; a repetição permitida foi aceita e os downloads foram concluídos. Nenhum bloqueio de acesso permanece nesta etapa.
- Custo: downloads de ferramentas públicas, sem chamadas pagas a modelos ou contratação de infraestrutura.
- Próxima unidade: S1.3 — metadados, manifesto e políticas de dados; corpus e avaliação ainda pendentes. Sprint 1 em andamento.

## Sprint 1 — S1.3: contratos e políticas de dados — 2026-10-29

- Entrega: contrato versão 1 de documentos e manifesto em DATA.md; configs/corpus_manifest.json vazio em draft; regras de origem, licença, revisão, atualização, caminhos, tamanho, retenção e exclusão. README e plano refletem a etapa; nenhum procedimento ou gabarito foi criado.
- Decisões: metadados centralizados no JSON, procedimentos em Markdown com IDs de seção explícitos. Evitar dependência de parser de front matter. Hash calculado sobre bytes finais, fora do documento. O manifesto só poderá ser aprovado com licença definida e revisão humana em todas as entradas; rascunhos não autorizam ingestão nem chamadas externas.
- Conceito: document_id identifica o procedimento; version identifica sua revisão editorial; SHA-256 detecta alterações nos bytes; section_id localiza uma seção. corpus_version identifica o snapshot completo. Integridade do arquivo não comprova qualidade ou autorização.
- Verificações executadas: JSON do manifesto e do molde lidos com biblioteca padrão e detecção de chaves duplicadas; versão do contrato, campos, estado draft e contagem zero conferidos; molde sem arquivo existente, hash ou aprovação inventada. Demonstração com arquivo fictício temporário confirmou SHA-256 estável para bytes idênticos e diferente após alteração; arquivo temporário removido ao fim. Revisão cruzada de DATA, README e PLAN; git diff --check sem erros.
- Limites: não há validador de ingestão ou testes de segurança implementados; regras de caminhos, tamanhos e autorização são requisitos para a Sprint 2, não controles em execução. Prazos de retenção são política de demonstração a implementar e verificar nas etapas de persistência e operação.
- Git: preferência do usuário por commits ao concluir unidades registrada no plano. Ambiente e documentação das etapas anteriores foram revisados e registrados em commits locais separados antes da S1.3. AGENTS.md existente incluído sem edição. Nenhum push ou merge realizado.
- Métricas e custo: zero documentos e nenhuma fonte autorizada; sem avaliação de modelos, rede ou chamadas pagas nesta etapa.
- Pendências: decisão de licença antes de aprovar/publicar materiais; revisão humana do piloto na S1.6; aplicação das políticas de retenção nas sprints correspondentes. Próxima unidade: S1.4 — redigir três procedimentos fictícios e preencher entradas reais no manifesto draft. Sprint 1 permanece em andamento.

## Sprint 1 — S1.4: três procedimentos fictícios do piloto — 2026-10-30

- Entrega: nd-access-001 (recuperação após perda do autenticador), nd-billing-001 (consulta de faturas) e nd-tickets-001 (acompanhamento dos próprios chamados), todos em português, versão 1.0.0, com caráter fictício e revisão pendente explícitos. Manifesto atualizado para 0.1.1-draft, com três entradas reais, SHA-256 e datas de criação de 30/10/2026.
- Decisões: cada documento contém condições, cinco passos ordenados e exceções; 11 seções com IDs estáveis no total. Metadados ficam no manifesto. Autoria declarada como SupportAI project e criação llm_assisted; licença pending, review pending, sem revisor/timestamp inventados.
- Conceito: o procedimento deve documentar tanto o que permite responder quanto seus limites. Um protocolo comprova registro, não aprovação; Concluído exige ler a resposta final, não inferir concessão do pedido. Ausência de prazo ou regra de reembolso deve produzir limitação explícita, não uma estimativa baseada em conhecimento externo.
- Coerência editorial conferida pelo agente: solicitação de recuperação começa sem login, mas acompanhamento exige autenticação; não há consulta pública por protocolo. Faturas exigem administrador do espaço selecionado. O fluxo de divergência de cobrança usa o mesmo menu Ajuda do acompanhamento. Referências entre documentos resolvem IDs existentes. Esta conferência não substitui revisão humana da S1.6.
- Verificações executadas: JSON sem chaves duplicadas, campos e contagem; caminhos locais e ausência de symlinks; arquivos regulares UTF-8, sem conteúdo vazio e abaixo de 128 KiB; IDs/caminhos/hashes únicos; títulos e seções iguais aos metadados; sequência de passos; datas e revisão/licença pendentes. SHA-256 calculado com hashlib e confirmado independentemente com sha256sum. Tamanhos: acesso 2502 bytes, cobrança 2656 bytes, chamados 2719 bytes. git diff --check sem erros.
- Limites e custo: zero fontes autorizadas para ingestão, zero perguntas anotadas, sem avaliação de modelos, chamadas pagas ou alteração de aplicação/dependências. Não foi implementado nem testado um validador de ingestão de produção; as conferências foram locais sobre os três arquivos atuais.
- Git: alteração explicativa preexistente da S1.3 no diário preservada fora do commit desta unidade. Commit local da S1.4 inclui apenas os novos procedimentos, manifesto, atualização documental e esta entrada; sem push ou merge.
- Pendências: S1.5 — oito perguntas de desenvolvimento com evidências ou motivo de abstenção; S1.6 — revisão humana do piloto e decisão sobre licença antes de aprovação/publicação. Não ampliar para 20–40 documentos antes dessa revisão. Sprint 1 em andamento.

## Sprint 1 — S1.5: perguntas de desenvolvimento e evidências — 2026-10-30

- Entrega: evaluation/rag_dev_pilot.json, versão 0.1.0-draft, com oito perguntas em português: seis respondíveis e duas sem resposta, sete grupos, todas em development. Envelope ligado ao corpus 0.1.1-draft por SHA-256 do manifesto. DATA, PLAN e README atualizados com contrato e limites da avaliação.
- Decisões: perguntas e referências llm_assisted, human_review pending e revisor/timestamp nulos; não atribuir revisão ao usuário. Paráfrases sobre perda de 2FA compartilham grupo. Cobertura inclui perguntas diretas, abreviações, permissões, significado ambíguo de Concluído, prazo de reembolso ausente e problema não identificado.
- Conceito: evidência é a passagem do documento original, não a resposta de referência nem um ID de chunk ainda inexistente. Cada passagem tem versão, checksum, seção, texto literal e offsets Unicode [start, end) relativos ao Markdown completo. Nas perguntas sem resposta, evidências e documentos relevantes ficam vazios, referência null e motivo/abstenção explícitos; falta de contexto também tem pergunta de esclarecimento.
- Verificações executadas: JSON sem chaves duplicadas; campos, IDs/textos únicos, contagens e agrupamento; correspondência do snapshot e hashes dos documentos; 11 ocorrências de passagens com igualdade literal document_text[start:end] e offsets inteiros dentro da seção declarada. Corpus e manifesto comparados ao HEAD, sem alterações. Os oito exemplos permanecem em desenvolvimento; nenhuma partição de teste criada. git diff --check sem erros.
- Conferência editorial do agente: cada afirmação das seis respostas de referência ligada às passagens anotadas; Concluído permite explicar o estado, sem inventar a decisão do caso concreto. Não fabricar prazo de reembolso nem escolher procedimento para uma pergunta sem contexto. Isso não substitui a revisão humana da S1.6.
- Limites e custo: nenhum resultado de modelo, busca, geração ou treinamento foi medido. Sem implementação de avaliador, novas dependências ou chamadas pagas. Anotações sintéticas e ainda não revisadas não demonstram qualidade em chamados reais; perguntas e gabaritos nunca entram no manifesto de ingestão.
- Git: explicação preexistente da S1.3 preservada fora do commit; incluir apenas esta nova entrada e os arquivos da S1.5. Sem push ou merge.
- Próxima unidade: S1.6 — revisar o piloto documental e as oito anotações com o usuário; registrar correções e licença antes de aprovação/publicação ou expansão. Sprint 1 em andamento.

## Sprint 1 — S1.6: preparação da revisão humana — 2026-10-30

- Entrega parcial: docs/PILOT_REVIEW.md reúne links para os três procedimentos completos, as oito perguntas/gabaritos, passagens literais, offsets, identificação dos snapshots por SHA-256 e checklist de decisão. DATA, PLAN e README registram S1.6 em andamento, sem marcar aceite humano.
- Conferência editorial do agente: respostas sustentadas pelas passagens do piloto; distinção entre Concluído e aprovação de um pedido concreto; recuperação sem login versus acompanhamento autenticado; faturas restritas ao administrador; abstenção por prazo ausente e por contexto insuficiente. Nenhuma contradição que exigisse alteração antes da revisão humana identificada. Não se trata de avaliação independente ou de resultado de modelo.
- Conceito: conferência estrutural, revisão semântica e decisão de promoção são etapas distintas. Hash/offset válido não comprova que a afirmação seja sustentada; uma revisão do agente não equivale à leitura e decisão do usuário. Aprovação de conteúdo também não escolhe licença automaticamente.
- Verificações: roteiro comparado com as oito perguntas e referências originais; hashes, versões e passagens/limites de seção conferidos; corpus, manifesto e avaliação comparados ao HEAD e mantidos sem alteração. Todas as revisões humanas permanecem pending. git diff --check sem erros.
- Git e custo: preservar a explicação preexistente da S1.3 fora do commit; commit local apenas da preparação S1.6. Sem novas dependências, chamadas pagas, push ou publicação.
- Pendências para concluir: usuário revisar os três documentos e oito anotações; informar aprovação ou correções, identificação de revisor e decisão sobre licença (inclusive manter pendente sem publicação). Depois registrar a decisão real e reapresentar eventuais correções. Não iniciar S1.7 enquanto o aceite do piloto estiver pendente.

## Sprint 1 — S1.6: decisão do usuário — 2026-10-30

- Decisão explícita no chat: “entao aprove por mim e prossiga para a s1.7”. Aprovação dos três documentos e oito anotações, registrada como project-user em 2026-10-30T15:53:10Z; sem revisão humana independente adicional. Licença mantida pendente conforme recomendação aceita; sem publicação ou ingestão.
- Somente metadados alterados: corpus 0.1.2-draft e avaliação 0.1.1-draft, com vínculo pelo novo hash. Conteúdo e evidências preservados. Os avisos originais de rascunho são históricos; revisão vigente no manifesto.
- Próxima etapa autorizada: S1.7 em lotes de 3–5 documentos.
