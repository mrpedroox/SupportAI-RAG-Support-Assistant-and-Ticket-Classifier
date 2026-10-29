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
