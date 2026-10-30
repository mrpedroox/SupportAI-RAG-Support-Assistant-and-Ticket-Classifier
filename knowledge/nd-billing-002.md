# Leitura dos campos de uma fatura

## scope — Escopo e condições

Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.

Exige Administrador do espaço de trabalho autenticado, conforme `nd-billing-001`. Explica os campos da fatura já localizada, sem consultar dados financeiros em tempo real pelo assistente.

## invoice-fields — Interpretar o registro exibido

1. Localize a fatura pelo mês e ano em **Configurações**, **Cobrança**, **Faturas**, conforme `nd-billing-001`.
2. Confira o **Identificador** da fatura. Use esse identificador ao relatar uma divergência; não o substitua pelo protocolo de um chamado.
3. Leia o **Período** exibido para identificar a referência do registro. Não o trate como prazo de pagamento.
4. Leia **Valor** e **Situação** exatamente como apresentados. O download é uma cópia do registro; não comprova pagamento ou ausência de débitos.
5. Se houver uma divergência, abra um chamado com identificador e descrição conforme `nd-billing-001` e `nd-tickets-002`, sem dados de cartão ou comprovantes com dados pessoais.

## exceptions — Exceções e encaminhamento

Se não conseguir identificar o significado de uma situação específica, peça esclarecimento ou revisão humana. A base não fornece um glossário de estados financeiros. Não converta o valor para outra moeda nem calcule encargos sem regra documentada.

## limits — Limites do procedimento

Não define vencimento, tributos, descontos, cálculo proporcional, quitação ou prazo de reembolso. Identificador de fatura não concede permissão de acesso a outra organização.
