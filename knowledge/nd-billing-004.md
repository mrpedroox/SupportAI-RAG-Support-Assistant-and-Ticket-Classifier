# Falha ao baixar uma fatura já localizada

## scope — Escopo e condições

Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.

Exige Administrador do espaço de trabalho autenticado e uma fatura existente já aberta conforme `nd-billing-001`. A falha de download não prova erro de cobrança nem altera a fatura.

## invoice-download-failure — Conferir e relatar a falha

1. Confira o espaço, o mês e o ano e abra a fatura desejada. Anote somente seu identificador para eventual relato.
2. Use **Baixar fatura** na tela da fatura. Confira a área de downloads do navegador antes de repetir a ação.
3. Se nenhum arquivo aparecer, observe se o navegador exibiu uma mensagem de bloqueio ou falha. Não desative proteções nem instale extensões por orientação deste procedimento.
4. Se a sessão tiver expirado, siga `nd-issues-002` e tente localizar novamente a mesma fatura após autenticar.
5. Se a falha persistir, registre um chamado com o identificador e a mensagem de erro sanitizada, conforme `nd-tickets-002`. Não envie o conteúdo financeiro completo ou dados de cartão.

## exceptions — Exceções e encaminhamento

Se a fatura não estiver na lista, siga a exceção de ausência de `nd-billing-001`; não classifique isso como falha de download. Se baixar um arquivo inesperado, não o trate como fatura válida e encaminhe a ocorrência à revisão humana.

## limits — Limites do procedimento

Não fornece link público, envio de fatura por e-mail ou acesso por outra conta. Não garante formato do arquivo, prazo de correção ou nova emissão de cobrança.
