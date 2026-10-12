# NAAP — Gestão da clínica

Linguagem do negócio do NAAP (Núcleo de Atendimento e Avaliação Psicológica, Fortaleza) usada no sistema interno de gestão: agenda, dinheiro, repasses e indicadores. O sistema não guarda conteúdo clínico.

## Pessoas e papéis

**Pessoa**:
Alguém, ou uma empresa, cadastrado uma única vez no NAAP e que pode acumular vários papéis.
_Evitar_: Cliente, cadastro

**Sócia**:
Uma das quatro donas do NAAP; atende pacientes e participa da divisão de lucros.
_Evitar_: Sócio, dona, proprietária

**Parceiro**:
Psicólogo de fora do quadro de sócias que atende pacientes do NAAP e recebe repasse.
_Evitar_: Terceirizado, colaborador, prestador

**Profissional**:
Quem atende pacientes do NAAP: uma sócia ou um parceiro.
_Evitar_: Terapeuta, psicólogo

**Paciente**:
Pessoa atendida pelo NAAP em psicoterapia, consulta ou avaliação.
_Evitar_: Cliente

**Responsável financeiro**:
Quem paga pelos atendimentos de um paciente e recebe a nota; pode ser o próprio paciente.
_Evitar_: Pagador, tomador

**Locatário**:
Pessoa que aluga sala, auditório ou teste psicológico do NAAP.
_Evitar_: Inquilino, parceiro (quando está alugando)

**Aluno**:
Pessoa inscrita em curso, formação ou mentoria do NAAP.
_Evitar_: Cursista, participante, mentorado

**Secretária**:
Funcionária que opera o dia a dia (agenda, cadastros, recebimentos, despesas e notas) sem ver o lucro nem a divisão de lucros.
_Evitar_: Recepção, recepcionista, atendente

**Convênio de desconto**:
Acordo com uma entidade, como a CAACE, que dá desconto aos associados dela; não é plano de saúde.
_Evitar_: Parceria (confunde com parceiro), convênio (sozinho)

## Atendimento

**Sessão**:
Um horário na agenda de um profissional reservado para um paciente.
_Evitar_: Atendimento, horário, consulta (no sentido genérico)

**Horário fixo**:
Dia e hora semanais de um paciente com um profissional, que geram as sessões até serem encerrados.
_Evitar_: Recorrência, horário recorrente

**Falta cobrada**:
Sessão a que o paciente não compareceu e que é cobrada como se tivesse sido realizada.
_Evitar_: No-show

**Falta abonada**:
Falta que não é cobrada nem usa crédito de sessões: falta cobrada que a secretária decidiu abonar, ou falta na 1ª consulta. Desmarcação com aviso no prazo não é falta.
_Evitar_: Falta justificada

**Contato**:
Registro de que uma pessoa procurou o NAAP para psicoterapia ou avaliação, com origem e situação, até virar paciente ou não fechar.
_Evitar_: Lead, interessado

**Origem**:
Canal por onde um contato chegou ao NAAP: Instagram, Google, site, indicação de paciente, indicação de profissional, CAACE ou outra.
_Evitar_: Fonte

**Avaliação**:
Serviço fechado de avaliação psicológica ou neuropsicológica, com várias sessões, que termina na entrega do laudo.
_Evitar_: Testagem, psicodiagnóstico (quando genérico)

**Crédito de sessões**:
Sessões pagas em pacote e ainda não realizadas.
_Evitar_: Saldo de sessões, sessões a haver

**Sessão avulsa**:
Sessão cobrada sozinha, fora de pacote: por falta de crédito de sessões ou por ser de avaliação vendida por sessão.
_Evitar_: Sessão extra

**Conflito a resolver**:
Data de horário fixo ou de aluguel fixo que não pôde entrar na agenda por choque ou por dia sem funcionamento.
_Evitar_: Erro de agenda

**Dia sem funcionamento**:
Feriado ou recesso cadastrado pelas sócias, em que horários fixos e aluguéis fixos não geram datas.
_Evitar_: Folga

**Lista de espera**:
Contatos e pacientes que aguardam um horário disponível com algum profissional.
_Evitar_: Fila, demanda reprimida

## Salas e aluguéis

**Sala**:
Espaço de atendimento do NAAP: sala de sócia, de uso exclusivo dela, ou sala de aluguel, usada por parceiros e locatários.
_Evitar_: Consultório

**Auditório**:
Espaço para cursos e eventos, alugado por diária ou bloqueado para uso do NAAP.
_Evitar_: Sala de eventos

**Turno**:
Cada bloco de horário configurado para aluguel de sala: hoje, 7h às 13h e 13h às 19h de segunda a sexta, e 6h às 12h no sábado.
_Evitar_: Período

**Aluguel**:
Uso pago de uma sala, do auditório ou de um teste por um locatário durante um período.
_Evitar_: Reserva, locação

**Bloqueio**:
Período em que uma sala ou o auditório fica separado para uso do próprio NAAP, como um curso.
_Evitar_: Reserva

## Dinheiro

**Linha de receita**:
Uma das sete famílias em que toda receita é classificada: Psicoterapia e consultas; Avaliação psicológica e neuropsicológica; Avaliação psicossocial para empresas; Supervisão; Aluguel de salas e auditório; Aluguel de testes; Cursos, formações e mentorias.
_Evitar_: Unidade de negócio, centro de receita, produto

**Serviço**:
O que o NAAP vende numa sessão ou avaliação, com linha de receita, preço de tabela, duração e tipo.
_Evitar_: Produto, procedimento

**Preço do paciente**:
Preço de um serviço para um paciente: o valor combinado dele, ou o preço de tabela menos o convênio de desconto.
_Evitar_: Preço da venda

**Pacote**:
Valor fechado pago por um conjunto de sessões ou por uma avaliação completa.
_Evitar_: Plano, mensalidade

**Lançamento simples**:
Receita registrada direto numa linha de receita, sem agenda nem pacote, como supervisão, aluguel de testes, cursos e avaliações para empresas.
_Evitar_: Receita avulsa, entrada manual

**Cobrança**:
Valor que uma pessoa deve ao NAAP, com vencimento; uma venda pode gerar várias, uma por parcela.
_Evitar_: Conta a receber, título, boleto

**Parcela**:
Cada cobrança de um pacote pago em partes.
_Evitar_: Prestação

**Conta**:
Lugar onde o dinheiro do NAAP fica: banco, caixa da secretária (dinheiro) ou maquininha.
_Evitar_: Carteira

**Recebimento**:
Dinheiro que entra no NAAP, com a forma usada: Pix, cartão, dinheiro ou transferência.
_Evitar_: Baixa, entrada, pagamento

**Pagamento**:
Dinheiro que sai do NAAP: despesa, guia do Simples, repasse, devolução ou divisão de lucros.
_Evitar_: Saída, baixa

**Transferência**:
Dinheiro que passa de uma conta do NAAP para outra, como o depósito do caixa no banco; não é recebimento nem pagamento.
_Evitar_: Depósito, sangria

**Cancelamento de recebimento**:
Desfazimento de um recebimento lançado errado, com motivo e data própria; a cobrança volta a ficar aberta.
_Evitar_: Estorno

**Devolução**:
Dinheiro devolvido a quem pagou por algo que não vai usar: crédito de sessões, avaliação interrompida ou data de aluguel cancelada no prazo.
_Evitar_: Estorno, reembolso

**Valor líquido**:
Valor de um recebimento já descontada a taxa da maquininha.

**Repasse**:
Parte do valor de um atendimento já realizado e recebido que o NAAP paga ao parceiro que atendeu; faltas cobradas também geram repasse, e na avaliação o repasse vem com a entrega do laudo.
_Evitar_: Comissão, split, honorário

**A liberar**:
Repasse de sessões já realizadas que só fica devido quando o responsável financeiro pagar.
_Evitar_: Repasse pendente

**Fechamento de repasse**:
Apuração mensal de quanto o NAAP deve a cada parceiro pelos atendimentos realizados e recebidos.
_Evitar_: Acerto, prestação de contas

**Custo direto**:
Gasto ligado a uma única linha de receita, como repasse, taxa de cartão ou material de teste.
_Evitar_: Custo variável

**Despesa geral**:
Gasto do NAAP como um todo, como aluguel do imóvel, secretária e contador; não é dividido entre linhas de receita.
_Evitar_: Custo fixo, rateio

**Faturamento**:
Total das receitas do mês: pacotes no mês da venda, sessões avulsas e aluguéis no mês do uso, lançamentos simples no mês informado; devoluções e cancelamentos entram negativos no mês em que acontecem.
_Evitar_: Receita bruta, vendas

**Resultado**:
Receitas do mês menos custos e despesas do mês, mesmo que ainda não recebidas ou pagas.
_Evitar_: DRE, lucro contábil

**Caixa do mês**:
O que de fato entrou e saiu das contas do NAAP no mês, sem contar transferências nem a divisão de lucros.
_Evitar_: Saldo (é o valor de uma conta num momento), caixa (sozinho, confunde com o caixa da secretária)

**Lucro a dividir**:
Caixa do mês trocando o repasse e a guia do Simples pagos no mês pelo repasse devido e pelo imposto estimado do próprio mês, ainda descontada a diferença entre cada guia paga e a estimativa dela.
_Evitar_: Sobra, lucro líquido

**Fundo de reserva**:
Percentual do lucro a dividir guardado antes da divisão de lucros.
_Evitar_: Reserva (sozinho), poupança

**Produção**:
Receita gerada pelos atendimentos de um profissional em um período.
_Evitar_: Faturamento da sócia

**Prejuízo a compensar**:
Lucro a dividir negativo de meses anteriores, descontado dos meses seguintes antes do fundo de reserva até zerar.
_Evitar_: Dívida, déficit

**Fechamento do mês**:
Registro da divisão de lucros de um mês, que congela os números dele; correções posteriores entram como ajuste no primeiro mês aberto.
_Evitar_: Fechamento (sozinho, confunde com o fechamento de repasse)

**Divisão de lucros**:
Distribuição do lucro a dividir, depois do fundo de reserva, entre as sócias por uma regra ajustável que começa em partes iguais.
_Evitar_: Rateio, dividendos, retirada

**Guia do Simples**:
Pagamento real do imposto do Simples (DAS) de um mês de competência; entra no caixa do mês, e a diferença em relação ao imposto estimado daquele mês entra no lucro a dividir.
_Evitar_: Guia de imposto, imposto pago

**Inadimplência**:
Valores vencidos e ainda não recebidos.
_Evitar_: Calote, dívida

**Data de início**:
Dia em que o NAAP passa a usar o sistema; o que veio antes entra só como saldos iniciais.
_Evitar_: Implantação, virada

**Nota**:
Nota fiscal de serviço (NFS-e) que a secretária emite para um recebimento; fica pendente até ser emitida e ganhar número.
_Evitar_: Recibo (é outro documento)
