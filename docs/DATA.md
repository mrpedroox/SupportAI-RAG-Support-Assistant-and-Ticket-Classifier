# SupportAI — Domínio e dados

Data: 28/10/2026. Etapa: S1.1 — delimitação do domínio.

O plano foi aprovado pelo usuário. Este documento concretiza o domínio proposto; não contém procedimentos operacionais nem gabaritos. Corpus e perguntas ainda não foram criados. Contratos de metadados, manifesto e políticas detalhadas serão definidos na S1.3.

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

- Corpus planejado: 24 documentos autorais fictícios, dentro da faixa de 20–40; começar com 3 para revisão do piloto.
- Origem planejada: elaboração para este projeto. Identificar assistência de LLM e revisão humana quando ocorrerem; nenhum texto gerado está automaticamente aprovado.
- Avaliação planejada: cerca de 50 perguntas revisadas, aproximadamente 35 de desenvolvimento e 15 de teste, com grupos de paráfrases preservados. As 8 perguntas do piloto ficarão em desenvolvimento.
- `knowledge/` conterá procedimentos; `evaluation/`, perguntas e gabaritos. Somente documentos autorizados por manifesto poderão ser indexados. Este documento de escopo também não é fonte de procedimentos para ingestão.
- Chamados supervisionados terão conjunto próprio na Sprint 7; perguntas RAG não substituem exemplos rotulados de classificação.
- Não coletar dados reais de clientes, credenciais ou documentos privados nesta etapa. Nenhum conteúdo será enviado a fornecedores externos na Sprint 1.
- Licenças de código e materiais autorais permanecem pendentes. Não atribuir licença por suposição nem tratar ficção como permissão automática de redistribuição.
- Política de atualização, retenção/exclusão, metadados e checksum ficam para S1.3. Não há dados operacionais armazenados neste momento.

## Orçamento e limitações

Orçamento inicial da Sprint 1: **R$ 0 em APIs e hospedagem**, com trabalho local e revisão manual. O orçamento de fornecedores será definido antes de chamadas pagas nas etapas seguintes.

O corpus fictício permite avaliar se o sistema segue procedimentos controlados; seus resultados não demonstram desempenho em chamados reais. Não haverá execução de ferramentas pelo LLM, fine-tuning de LLM ou treinamento de embeddings no MVP.

## Verificação da S1.1

Revisão documental realizada: os três casos cobertos correspondem às áreas declaradas; os três casos fora da cobertura têm motivos explícitos. Não foram definidos passos de recuperação, permissões concretas ou regras de cobrança sem documentos. O corpus, o conjunto de avaliação e a aplicação continuam inexistentes.

Após a S1.1, o ambiente mínimo foi preparado na S1.2. Próxima unidade: S1.3 — definir metadados, manifesto autorizado e políticas de dados.
