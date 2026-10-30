# Botão de envio indisponível no formulário de chamado

## scope — Escopo e condições

Procedimento fictício do NexoDesk, criado para o SupportAI. Rascunho sem revisão humana; não descreve um serviço real. A versão, a origem e a situação de licença constam no manifesto do corpus.

Aplica-se ao formulário Novo chamado de `nd-tickets-002`, com conta autenticada. Assunto e Descrição são obrigatórios; espaços em branco sozinhos não preenchem esses campos. Não se aplica ao envio de respostas ou recuperação de conta.

## ticket-form-validation — Conferir campos obrigatórios

1. Confira que está em **Ajuda**, **Novo chamado**, na conta e no espaço corretos.
2. Verifique se **Assunto** e **Descrição** têm texto útil. Preencha o resumo e os detalhes necessários sem senhas, códigos ou dados pessoais de terceiros.
3. Leia mensagens de validação próximas aos campos e ajuste apenas os valores indicados, respeitando os limites exibidos.
4. Se **Enviar** ficar disponível, confira o conteúdo e envie uma única vez, aguardando protocolo conforme `nd-tickets-002`.
5. Se continuar indisponível sem mensagem que permita corrigir o formulário, registre a limitação e o texto do erro sanitizado para revisão humana. Não altere controles internos do navegador para forçar o envio.

## exceptions — Exceções e encaminhamento

Se aparecer sessão expirada, siga `nd-issues-002`. Se não souber descrever o problema, peça esclarecimento; não preencha textos inventados somente para habilitar o botão. Não abra outro chamado duplicado enquanto não houver confirmação do resultado.

## limits — Limites do procedimento

Não define quantidade máxima de caracteres ou causa técnica de bloqueio não explicada pela tela. Corrigir o formulário não garante resolução do chamado, prioridade ou prazo de atendimento.
