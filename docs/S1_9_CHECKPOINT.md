# SupportAI — S1.9: auditoria e checkpoint da Sprint 1

Data: 30/10/2026. Snapshot auditado: dados registrados até o commit `de9931a`; a S1.9 altera somente documentação. Auditoria local e explicação concluídas. **Sprint 1 permanece aberta:** licença pendente impede autorizar o manifesto de ingestão; a compreensão do usuário ainda precisa ser demonstrada. Não há aplicação ou modelo avaliado.

## O que ficou pronto

| Material | Versão e resultado conferido |
| --- | --- |
| Corpus | `0.1.5-draft`: 24 documentos fictícios em português, 95 seções e 44 referências internas resolvidas. Conteúdo aprovado; 24 licenças pendentes. |
| Desenvolvimento RAG | `0.2.1-draft`: 35 perguntas, 28 respondíveis e 7 sem resposta, 22 grupos. As oito anotações do piloto continuam intactas aqui. |
| Teste RAG | `0.2.1-draft`: 15 perguntas, 11 respondíveis e 4 sem resposta, 11 grupos. Versão e checksum congelados na S1.8. |
| Evidências | 73 ocorrências de passagens literais dentro das seções declaradas, com versão/hash e offsets Unicode `[start, end)`. Os 24 documentos aparecem como evidência. |
| Aceite das anotações | 50 aprovadas; as 42 novas foram revisadas/aprovadas pelo agente por autorização explícita do usuário. Não há revisão humana independente. |
| Separação | 33 grupos sem interseção entre partições; nenhum gabarito é fonte do manifesto de ingestão. |
| Ambiente | CPython 3.12.16, uv 0.13.6, pytest 9.2.0 e Ruff 0.17.3. Um único `uv.lock`; validação offline e compatibilidade das dependências aprovadas. |

O teste foi lido pelo agente para anotação/revisão antes do congelamento. Essa exposição está declarada no manifesto; não há resultado de modelo nem ajuste de configuração com essas perguntas. Nesta auditoria, o arquivo foi verificado estruturalmente e por checksum, sem executar modelo ou usar seus casos para orientar implementação. As futuras escolhas de chunking, busca, prompt, top-k e limiares devem usar desenvolvimento. Os conjuntos são pequenos e fictícios; não demonstram desempenho em chamados reais.

## Evidências da auditoria

Foram conferidos JSON sem chaves duplicadas, contagens, IDs/textos únicos, agrupamento e contratos de resposta/abstenção/revisão; 73 correspondências literais e limites de seção; versões e checksums dos documentos; caminhos locais sob `knowledge/`, arquivos regulares UTF-8 não vazios de até 128 KiB, sem symlinks; títulos/seções e referências internas. SHA-256 dos documentos foi confirmado também com `sha256sum`. Piloto, corpus e arquivos de avaliação permaneceram idênticos ao commit auditado.

O checksum do teste coincide com a partição e com `test_freeze`. Os hashes desta auditoria são:

| Arquivo | SHA-256 |
| --- | --- |
| `configs/corpus_manifest.json` | `ee49671201906bdcb9210587739cfca0fe167ed710dd6af36d798e03fea13389` |
| `evaluation/rag_split_manifest.json` | `33ffd7531ce939e05c53ac7f08072fed8e3c4749ca7a36233934d6c8c1fd672d` |
| `evaluation/rag_development.json` | `7e5c49063586adaec2059a674e42cd19973bfa0fac1b069e2e709ca8a80ee5f9` |
| `evaluation/rag_test.json` | `7586e2c45c3d681b938f23a331846c5b15cf71b0c5eb3bfcacd23b67a7eb662c` |

A lista de arquivos rastreados não contém `.env`, ambientes, cache, modelos, tracking ou dados brutos. Seis caminhos fictícios foram consultados com `git check-ignore`, sem criar arquivos: `.env`, `data/raw/probe.json`, `models/probe.joblib`, `mlruns/probe`, `backups/probe` e `.cache/probe`. Todos ficaram cobertos pelas regras. Essa conferência não é uma varredura completa de segredos nem valida dados privados que não foram coletados.

Comandos de conferência que podem ser repetidos na raiz:

```bash
.venv/bin/python --version
.venv-tools/bin/uv --version
.venv/bin/pytest --version
.venv/bin/ruff --version
.venv-tools/bin/uv lock --check --offline
.venv-tools/bin/uv pip check --python .venv/bin/python
sha256sum configs/corpus_manifest.json evaluation/rag_split_manifest.json evaluation/rag_development.json evaluation/rag_test.json
git diff --check
```

As versões devem coincidir com a tabela; os hashes, com os registrados acima. Lock, compatibilidade e espaços em branco foram verificados com sucesso. A recriação offline de um ambiente vazio pertence à S1.2 e não foi repetida nesta etapa. Não há suíte de aplicação: consultar a versão do pytest não equivale a executar testes. As conferências dos dados foram locais, com biblioteca padrão do Python e ferramentas do sistema; o validador de ingestão de produção ainda será implementado na Sprint 2.

## Explicação e exercício manual

O corpus é o material de consulta: por exemplo, `knowledge/nd-access-001.md` descreve a recuperação após perda do autenticador. A avaliação guarda perguntas e o resultado esperado para medir o sistema; por isso seus gabaritos ficam fora da ingestão. Recuperação seleciona as passagens; geração usa essas evidências para redigir uma sugestão. A existência de uma citação não garante que ela sustente toda a resposta.

RAG fornece contexto a um modelo existente. O classificador obrigatório será outro fluxo: ajustar TF-IDF e regressão logística a chamados rotulados, avaliar e servir o pipeline. As 50 perguntas RAG não são seu dataset de treinamento.

Exercício usando apenas desenvolvimento, sem abrir o teste reservado:

1. Leia o documento `nd-access-001` e o caso `rag-dev-001` de `evaluation/rag_development.json`. Localize a passagem que distingue registro da solicitação de aprovação da recuperação.
2. Explique com suas palavras quais arquivos são conhecimento consultável e quais guardam o gabarito; o que faria a recuperação e o que faria a geração; e por que esse fluxo não treina o classificador.
3. Compare com `rag-dev-007`: justifique a abstenção sobre prazo de reembolso sem inventar uma regra ausente.

O agente conferiu as relações de evidência, mas não atribui ao usuário a execução do exercício. A explicação do usuário será registrada quando recebida; até lá, o critério de compreensão permanece aberto.

## Orçamento, limites e próximo passo

Orçamento da Sprint 1: **R$ 0 em APIs e hospedagem**. Nenhuma chamada paga ou contratação foi realizada nas etapas registradas; não se contabilizam assinatura da ferramenta, equipamento, energia ou tempo de trabalho. Não houve ingestão, embeddings, busca executada, geração pelo produto, treinamento, MLflow em execução, banco, API, Docker, CI ou deploy. Métricas de qualidade, latência, tokens e custo operacional permanecem não medidas.

Há **zero fontes autorizadas para ingestão**: o manifesto continua `draft` e as licenças continuam `pending`. A S1.9 documenta essa pendência e não escolhe licença nem promove um snapshot. A aprovação editorial é distinta da autorização de uso/publicação. Após decisão explícita de licença, a promoção deverá produzir outro snapshot e tratar o vínculo da avaliação de modo explícito; não se deve reescrever o teste congelado ou trocar seu hash silenciosamente.

O próximo marco técnico é a Sprint 2, começando pela validação administrativa local dos dados e pela preparação incremental do PostgreSQL/pgvector. Embeddings externos dependem também de fornecedor, política de dados e teto de gasto definidos antes de chamadas pagas. A Sprint 2 não foi iniciada. O checkpoint não encerra a Sprint 1 enquanto faltarem autorização do manifesto e demonstração de compreensão conforme [os critérios do plano](PLAN.md#45-critérios-de-aceite-da-sprint-1).
