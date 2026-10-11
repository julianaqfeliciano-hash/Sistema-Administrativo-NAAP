# Fase 1 — Sistema de gestão do NAAP

Status: rascunho para revisão das sócias (11/10/2026)

Termos em **negrito** seguem o [glossário](../../GLOSSARY.md). Decisões de arquitetura: [ADR-0001](../adr/0001-sem-conteudo-clinico.md) (sem conteúdo clínico), [ADR-0002](../adr/0002-agenda-origem-das-cobrancas.md) (agenda como origem das cobranças e repasses) e [ADR-0003](../adr/0003-nextjs-vercel-neon-sao-paulo.md) (tecnologia e hospedagem).

## Problema

O NAAP fatura de forma relevante com sete linhas de receita, quatro sócias e psicólogos parceiros, mas não tem nenhum sistema de gestão. Hoje a secretária controla a agenda e os pacientes numa planilha de Excel. Por isso as sócias:

1. não sabem o lucro real do mês nem quanto rende cada linha de receita;
2. calculam à mão o repasse dos parceiros;
3. não têm controle confiável de quem pagou, de quem está devendo e do caixa.

Também não sabem de onde vêm os pacientes novos, quanta demanda fica sem atendimento, quanto as salas de aluguel e o auditório ficam ocupados, nem quais notas fiscais faltam emitir.

## Solução

Um sistema web interno, com login, que funciona no computador e no celular, usado pelas quatro **sócias** e pela **secretária**. Parceiros, pacientes e o contador não acessam o sistema.

- A **agenda** do sistema passa a ser a única agenda do NAAP: **sessões**, **aluguéis** de sala e auditório e **bloqueios**, sem choque de sala nem de profissional.
- Cada **sessão** realizada (ou **falta cobrada**) usa o **crédito de sessões** do **pacote** ou gera uma **cobrança** avulsa, e alimenta o **repasse** do **parceiro**.
- Todas as receitas entram no sistema: pacotes, sessões avulsas, aluguéis e **lançamentos simples** para supervisão, aluguel de testes, cursos e empresas.
- A secretária registra **recebimentos** (com taxa da maquininha e **valor líquido**), controla as **notas**, lança despesas e fecha o repasse do mês, com extrato em PDF e mensagem pronta no WhatsApp.
- O painel responde, em um clique, às perguntas da operação. As sócias veem também **faturamento**, **resultado**, lucro por linha, **caixa**, **lucro a dividir** e **divisão de lucros**.
- Os **contatos** novos são registrados com **origem** até virarem pacientes ou não fecharem. A demanda não atendida fica na **lista de espera**.
- O sistema não guarda nenhum conteúdo clínico.

## Histórias de usuário

### Acesso e segurança

1. Como sócia, quero criar, desativar e redefinir a senha dos logins das sócias e da secretária, para controlar quem acessa o sistema sem depender de e-mail automático.
2. Como usuária, quero entrar com e-mail e senha e ser desconectada depois de 8 horas sem uso, para proteger os dados dos pacientes num computador compartilhado.
3. Como sócia, quero que a secretária não consiga ver faturamento total, resultado, lucro por linha, produção, lucro a dividir, divisão de lucros, configurações nem histórico de alterações, mesmo digitando o endereço da página, para manter essas informações só com as sócias.
4. Como sócia, quero ver o histórico de quem criou, alterou ou apagou sessões, aluguéis, pacotes, cobranças, recebimentos, despesas, repasses e configurações, e quando, para confiar nos números.
5. Como sócia, quero exportar todos os dados para Excel, para guardar uma cópia por conta própria.

### Cadastros

6. Como secretária, quero cadastrar uma **pessoa** uma única vez e dar a ela um ou mais papéis (**paciente**, **responsável financeiro**, **parceiro**, **locatário**, **aluno**), para não duplicar cadastros.
7. Como secretária, quero indicar o responsável financeiro de cada paciente (o próprio paciente, um familiar ou outra pessoa), para que a cobrança e a nota saiam no nome de quem paga.
8. Como secretária, quero registrar o **convênio de desconto** e o valor combinado de um paciente, para que os preços já saiam certos.
9. Como secretária, quero buscar pessoas por nome, telefone ou CPF, para achar o cadastro rápido.
10. Como secretária, quero ver na ficha do paciente os **horários fixos**, o crédito de sessões, as cobranças em aberto, os recebimentos e as próximas sessões, para atender sem procurar em vários lugares.
11. Como sócia, quero cadastrar os parceiros com o percentual de repasse (padrão de 30%) e se mandam nota ou RPA, para o repasse sair calculado.
12. Como sócia, quero cadastrar os serviços com linha de receita, preço, duração e tipo, para que agenda e cobrança usem a tabela.
13. Como sócia, quero cadastrar as **salas** (de sócia, de aluguel e o **auditório**), o horário de funcionamento e os **turnos**, para que agenda, aluguel e ocupação reflitam a clínica.
14. Como sócia, quero configurar preços de aluguel, alíquota estimada do imposto, regra de divisão de lucros, **fundo de reserva**, **contas**, taxas da maquininha por forma de pagamento, categorias de despesa, origens, convênios de desconto, chave Pix, endereço e textos das mensagens de WhatsApp, para ajustar o sistema sem programador.
15. Como sócia, quero cadastrar o horário de atendimento de cada profissional, para medir a ocupação da agenda dele.

### Agenda

16. Como secretária, quero ver a grade do dia e da semana por sala (com uma coluna "online") e por profissional, para saber o que está ocupado.
17. Como secretária, quero marcar uma sessão escolhendo paciente, profissional, serviço, sala ou online, data e hora, para organizar os atendimentos.
18. Como secretária, quero que o sistema não deixe usar a mesma sala, nem marcar o mesmo profissional, duas vezes no mesmo horário, mesmo que duas pessoas marquem ao mesmo tempo, para evitar choques.
19. Como secretária, quero criar um horário fixo semanal para um paciente, para que as sessões apareçam sozinhas nas próximas semanas até eu encerrar.
20. Como secretária, quero ver os "conflitos a resolver" quando uma sessão de horário fixo não puder ser criada, para resolver antes do dia.
21. Como secretária, quero remarcar uma sessão para outro dia, hora ou sala, para acomodar pedidos.
22. Como secretária, quero marcar a sessão como realizada, para que crédito, cobrança e repasse se atualizem sozinhos.
23. Como secretária, quero registrar que o paciente não compareceu, para que a sessão vire falta cobrada.
24. Como secretária, quero registrar que o paciente desmarcou, informando quando ele avisou, para que o sistema aplique a regra das 24 horas.
25. Como secretária, quero abonar uma falta cobrada, para não cobrar quando houver motivo aceito.
26. Como secretária, quero cancelar uma sessão, indicando se foi o paciente ou a clínica, para liberar o horário.
27. Como secretária, quero desfazer o registro de uma sessão marcada por engano, para corrigir sem bagunçar crédito, cobrança e repasse.
28. Como secretária, quero ver as sessões de dias passados que ainda estão como "agendada", para não esquecer de registrar o que aconteceu.
29. Como secretária, quero abrir o WhatsApp com o lembrete ou a confirmação já escritos para o paciente, para avisar com um clique.
30. Como secretária, quero abrir o WhatsApp com a agenda do dia ou da semana de um parceiro, para ele saber seus atendimentos sem acessar o sistema.

### Aluguéis e bloqueios

31. Como secretária, quero registrar um aluguel avulso de sala de aluguel por hora ou por turno, ou do auditório por diária, para cobrar o locatário.
32. Como secretária, quero registrar um aluguel fixo semanal, para que a cobrança do mês seja gerada sozinha com as datas daquele mês.
33. Como secretária, quero ajustar o valor de um aluguel específico, para casos negociados.
34. Como secretária, quero cancelar um aluguel ou encerrar um aluguel fixo, para liberar a sala.
35. Como secretária, quero bloquear o auditório ou uma sala para uso do NAAP, por exemplo um curso, para que ninguém alugue esse horário.

### Pacotes e cobranças

36. Como secretária, quero vender um pacote de sessões com quantidade, valor e parcelas, para registrar o pagamento adiantado.
37. Como secretária, quero que cada sessão realizada ou falta cobrada use uma sessão do crédito, e que a falta abonada não use, para o crédito ficar sempre certo.
38. Como secretária, quero ser avisada quando o crédito de um paciente acabar, para vender um novo pacote.
39. Como secretária, quero que uma sessão feita sem crédito vire cobrança avulsa e que, se ainda não estiver paga, eu possa incluí-la no próximo pacote vendido, para não perder nem duplicar cobrança.
40. Como secretária, quero vender uma **avaliação** como pacote completo, com parcelas, ou cobrar sessões avulsas de avaliação, para refletir as duas formas de venda.
41. Como secretária, quero registrar a entrega do laudo, para concluir a avaliação e liberar o repasse do parceiro.
42. Como secretária, quero fazer lançamentos simples (supervisão, aluguel de testes, cursos, avaliação psicossocial para empresas) na linha de receita certa, para que todas as receitas estejam no sistema já na Fase 1.
43. Como secretária, quero registrar a **devolução** de crédito de sessões a um paciente, para que faturamento e caixa mostrem o dinheiro devolvido.
44. Como secretária, quero ver todas as cobranças com vencimento, filtrando por pessoa, linha e situação, para cobrar em dia.
45. Como secretária, quero ver as cobranças vencidas e abrir o WhatsApp com a mensagem de cobrança pronta (valor, vencimento e chave Pix), para reduzir a **inadimplência**.

### Recebimentos e notas

46. Como secretária, quero registrar um recebimento (data, valor, forma, conta), com a taxa da maquininha calculada pela forma, para saber o valor líquido.
47. Como secretária, quero registrar recebimentos parciais de uma cobrança, para pagamentos em partes.
48. Como secretária, quero que todo recebimento comece com nota pendente e eu possa informar o número e a data da nota emitida, para não esquecer nenhuma.
49. Como secretária, quero uma lista das notas pendentes com os dados de quem paga (nome, CPF ou CNPJ, endereço, e-mail, valor e descrição), para emitir no Emissor Nacional rápido.
50. Como secretária, quero estornar um recebimento lançado errado, com motivo, para corrigir os números com rastro no histórico.

### Repasse

51. Como secretária, quero ver, a qualquer momento do mês, o repasse devido a cada parceiro e o que está "a liberar", para responder aos parceiros.
52. Como secretária, quero fechar o repasse do mês, congelando o valor de cada parceiro, para pagar com segurança.
53. Como secretária, quero registrar o pagamento do repasse de cada parceiro (data e conta), para o caixa ficar certo.
54. Como secretária, quero gerar o extrato do repasse em PDF e abrir o WhatsApp com o resumo, para o parceiro conferir.
55. Como parceiro, quero receber um extrato com cada atendimento (data, paciente, serviço, valor-base, percentual e repasse), os ajustes e o que está a liberar, para conferir meu pagamento.
56. Como sócia, quero que mudanças feitas depois de um **fechamento de repasse** (falta abonada tarde, pagamento atrasado, laudo) entrem como ajuste no fechamento seguinte, para nunca mexer num mês já pago.

### Despesas, contas e caixa

57. Como secretária, quero lançar despesas com categoria, valor, vencimento, mês de competência, fornecedor e, quando for **custo direto**, a linha de receita, para separar custos diretos de **despesas gerais**.
58. Como secretária, quero cadastrar despesas que se repetem todo mês, para que sejam criadas sozinhas até eu encerrar.
59. Como secretária, quero registrar o pagamento de uma despesa (data e conta), para o caixa ficar certo.
60. Como secretária, quero lançar a **guia de imposto** com o mês a que ela se refere, para o sistema trocar a estimativa pelo valor real.
61. Como secretária, quero registrar **transferências** entre contas (como o depósito do dinheiro do caixa no banco), para os saldos baterem.
62. Como secretária, quero ver o saldo de cada conta, para conferir com o banco, o caixa e a maquininha.
63. Como secretária, quero ver as despesas a pagar nos próximos 30 dias, para não atrasar contas.

### Contatos e lista de espera

64. Como secretária, quero registrar cada contato novo com origem, telefone e o que procura, para medir de onde vêm os pacientes.
65. Como secretária, quero agendar a 1ª consulta a partir do contato, sem redigitar dados.
66. Como secretária, quero que o contato vire paciente quando a 1ª consulta for realizada, para a conversão ser contada sozinha.
67. Como secretária, quero marcar um contato como "não fechou", com o motivo, para entender por que perdemos pacientes.
68. Como secretária, quero colocar contatos e pacientes na lista de espera com o horário e o tipo de profissional de que precisam, para chamar quando abrir vaga.
69. Como sócia, quero ver o tamanho da lista de espera e há quanto tempo as pessoas esperam, para decidir se chamamos mais parceiros.

### Painel, fechamento do mês e relatórios

70. Como secretária, quero um painel de operação com, no mês:
    - sessões realizadas, faltas e cancelamentos;
    - ocupação das salas de aluguel e do auditório, e ocupação da agenda de cada profissional;
    - contatos por origem e conversão, e lista de espera;
    - a receber e a pagar nos próximos 30 dias, inadimplência e saldo das contas;
    - crédito de sessões e notas pendentes.
71. Como sócia, quero ver faturamento, resultado e lucro de cada linha de receita, mês a mês, para saber o que rende.
72. Como sócia, quero ver a **produção** de cada profissional, para acompanhar quem gera receita.
73. Como sócia, quero ver o caixa do mês e o lucro a dividir com a conta explicada, para dividir só o dinheiro que é nosso.
74. Como sócia, quero ver a divisão de lucros do mês pela regra configurada, depois do fundo de reserva, e registrar os pagamentos às sócias.
75. Como sócia, quero ver os pacientes novos, ativos e que pararam de vir no mês, para acompanhar a retenção.
76. Como sócia, quero ver os indicadores dos últimos 12 meses lado a lado, para enxergar tendências.
77. Como secretária, quero gerar o relatório mensal do contador em Excel (recebimentos com notas, repasses, despesas pagas e resumo), para enviar todo mês.

### Começo do uso

78. Como sócia, quero baixar uma planilha-modelo de pacientes, preencher com os dados do Excel atual e importar vendo uma prévia antes de confirmar, para não digitar tudo de novo.
79. Como sócia, quero informar a data de início e os saldos iniciais, para começar com os números certos sem importar histórico. Os saldos iniciais são:
    - o saldo de cada conta;
    - as cobranças e despesas em aberto;
    - o crédito de sessões dos pacotes já pagos.
80. Como secretária, quero cadastrar os horários fixos ativos dos pacientes no começo, para a agenda já nascer pronta.

## Decisões de implementação

### Arquitetura

- **Um único projeto web** em Next.js com TypeScript. As telas são feitas no próprio servidor, e cada ação da usuária chama um **caso de uso** no servidor. A hospedagem é na Vercel (plano Pro), com as funções rodando em São Paulo. O banco é PostgreSQL no Neon, também em São Paulo. Detalhes no ADR-0003.
- **Três camadas:**
  - **Regras**: funções puras de agenda, crédito, preço, repasse e apuração, sem banco e sem tela.
  - **Casos de uso**: a interface do sistema. Cada ação (marcar sessão, vender pacote, registrar recebimento, fechar repasse etc.) confere a permissão, aplica as regras e grava tudo numa única transação.
  - **Telas**: só chamam casos de uso e consultas, nunca o banco direto.
- **Valores e datas:**
  - Dinheiro é guardado em centavos inteiros.
  - Percentuais são aplicados sobre valores acumulados e arredondados ao centavo; o valor do mês é a diferença entre dois acumulados, então nenhum centavo se perde.
  - Todas as datas e horas usam o fuso de Fortaleza (America/Fortaleza).
- **Migrações:** o banco é alterado só por migrações versionadas, aplicadas automaticamente na publicação.
- **Rotina diária** agendada (cron da Vercel). Cada passo pode rodar de novo sem duplicar nada:
  - estende os horários fixos até 12 semanas à frente;
  - no dia 1º, gera as cobranças dos aluguéis fixos do mês e as despesas recorrentes do mês.
- **Login** com better-auth: e-mail e senha, sem cadastro público, e sessão que expira após 8 horas sem uso. Só uma sócia cria contas e redefine senhas; não há envio de e-mail.

### Perfis e permissões

| O quê | Sócia | Secretária |
|---|:-:|:-:|
| Agenda, aluguéis, bloqueios, pessoas, contatos, lista de espera | ✓ | ✓ |
| Pacotes, cobranças, recebimentos, notas, devoluções | ✓ | ✓ |
| Despesas, contas, transferências, saldos | ✓ | ✓ |
| Repasse: apuração, fechamento, pagamento, extrato | ✓ | ✓ |
| Painel de operação, relatório do contador, importação de pacientes | ✓ | ✓ |
| Faturamento total, resultado, lucro por linha, produção, caixa do mês, lucro a dividir, divisão de lucros | ✓ | — |
| Configurações, usuários, saldos iniciais, histórico de alterações, exportar tudo | ✓ | — |

A permissão é conferida em cada caso de uso e em cada consulta no servidor, não só escondendo botões. Parceiros, pacientes e o contador não têm login.

### Cadastros

- **Pessoa:** nome, CPF ou CNPJ, telefone com WhatsApp, e-mail, data de nascimento, endereço (para a nota) e observações administrativas. Uma pessoa pode ter vários papéis. Como paciente, tem:
  - um responsável financeiro, que é uma pessoa e pode ser ela mesma;
  - opcionalmente um convênio de desconto;
  - opcionalmente valores combinados por serviço;
  - a origem, quando veio de um contato.
- **Profissional:** é uma sócia ou um parceiro, e pode ter horário de atendimento semanal.
  - A sócia tem a sua sala de sócia.
  - O parceiro tem percentual de repasse (padrão de 30%) e a indicação de se manda nota ou RPA, que serve só de informação para o contador.
- **Serviço:** nome, linha de receita, preço de tabela, duração padrão e tipo:
  - "sessão" (psicoterapia e consultas);
  - "avaliação em pacote" (sem sessões contadas);
  - "sessão avulsa de avaliação".
- **Preço de uma venda:** usa o valor combinado do paciente para aquele serviço, quando existe. Se não existe, usa o preço de tabela menos o percentual do convênio de desconto. O preço pode ser editado na venda, e a edição fica no histórico.

### Agenda

- **Choques:** três tipos de compromisso ocupam sala: sessão com sala, aluguel e bloqueio. Uma sala não pode ter dois compromissos sobrepostos, e um profissional não pode ter duas sessões sobrepostas; cancelados não contam. A garantia fica no próprio banco, por restrição de exclusão sobre uma tabela única de ocupações, e não só na tela. Assim, duas marcações ao mesmo tempo nunca passam.
- **Uso das salas:**
  - A sala de sócia só recebe sessões da própria sócia.
  - Parceiros e locatários usam as salas de aluguel; o auditório só recebe aluguel por diária e bloqueio.
  - Sócias também podem usar salas de aluguel.
  - Sessão online não tem sala e só confere o choque de profissional.
- **Situações da sessão:** agendada, realizada, falta cobrada, falta abonada ou cancelada (pelo paciente ou pela clínica). As mudanças possíveis:
  - "Não compareceu" vira falta cobrada.
  - "Desmarcou" pede quando o paciente avisou. Aviso com menos de 24 horas (ajustável) antes do início vira falta cobrada; com mais, vira cancelada.
  - Uma falta cobrada pode ser abonada.
  - "Desfazer" volta uma sessão realizada, com falta ou cancelada para agendada, e reverte os efeitos dela no crédito, na cobrança e no repasse. Uma sessão cancelada só volta se a sala e o profissional ainda estiverem livres. Tudo fica no histórico.
- **Remarcar** muda data, hora, sala ou profissional de uma sessão agendada, conferindo de novo os choques.
- **Horário fixo:** paciente, profissional, serviço, sala ou online, dia da semana, hora e data de início.
  - Gera as sessões até 12 semanas à frente.
  - Uma data com choque não é criada e vai para "conflitos a resolver".
  - Encerrar o horário fixo apaga as sessões futuras que ainda estão agendadas.
- **Pendências de registro:** a agenda mostra as sessões de dias passados que ainda estão como agendadas.

### Pacotes e crédito de sessões

- **Pacote de sessões** (linha "Psicoterapia e consultas"): paciente, responsável financeiro, serviço, profissional, quantidade de sessões, valor total, data da venda e parcelas (uma ou mais, com vencimentos).
  - O valor total sugerido é o preço da venda vezes a quantidade.
  - O valor de cada sessão do pacote é o valor total dividido pela quantidade.
- **Uso do crédito:**
  - Uma sessão de serviço do tipo "sessão", ao virar realizada ou falta cobrada, usa uma sessão do pacote mais antigo do paciente que ainda tem crédito.
  - Falta abonada e cancelamento não usam crédito; desfazer devolve.
  - O crédito não vence.
- **Sem crédito:** a sessão gera faturamento e cobrança avulsos no preço da venda, vencendo no dia da sessão, e o sistema avisa "crédito esgotado".
  - Ao vender o pacote seguinte, a secretária pode incluir as sessões avulsas ainda não pagas daquele paciente.
  - Nesse caso as cobranças e o faturamento avulsos delas são cancelados, e elas passam a usar o pacote novo.
- **Avaliação em pacote:** paciente, responsável financeiro, profissional, valor total, data da venda e parcelas.
  - As sessões da avaliação ficam ligadas a ela e não usam crédito nem geram cobrança.
  - A avaliação termina quando a secretária registra "laudo entregue", com a data.
  - A avaliação vendida por sessão usa o serviço "sessão avulsa de avaliação" (R$ 350), cobrado como sessão avulsa.
- **Devolução:** retira sessões não usadas do pacote. Gera um pagamento ao paciente (conta e data) e um faturamento negativo na linha, no mês da devolução. O valor sugerido é o valor por sessão vezes as sessões devolvidas, e pode ser editado.

### Faturamento e cobranças

**Faturamento** (em que mês a receita conta):

| Origem | Mês em que conta | Cobranças geradas | Vencimento |
|---|---|---|---|
| Pacote de sessões ou avaliação em pacote | mês da venda, pelo valor total | uma por parcela | definido na venda (padrão: data da venda) |
| Sessão avulsa ou falta cobrada sem crédito | mês da sessão | uma | dia da sessão |
| Aluguel avulso | mês do uso | uma | dia do uso |
| Aluguel fixo | mês das datas | uma por mês, criada no dia 1º | dia 5 do mês (ajustável) |
| Lançamento simples | mês informado | uma | data informada |
| Devolução | mês da devolução (valor negativo) | nenhuma | — |

- **Quem é cobrado:** o responsável financeiro do paciente, o locatário, ou a pessoa ou empresa informada no lançamento simples.
- **Situação da cobrança:** aberta, paga (quando a soma dos recebimentos chega ao valor) ou cancelada. Uma cobrança aberta com vencimento passado conta como inadimplência.

### Recebimentos e notas

- **Recebimento:** cobrança, data, valor bruto, forma (Pix, dinheiro, transferência, cartão de débito ou de crédito), conta, taxa e valor líquido.
  - A conta é sugerida pela forma: Pix e transferência vão para o banco, dinheiro para o caixa, cartão para a maquininha.
  - A taxa é o percentual configurado para a forma, e pode ser editada.
  - Recebimentos parciais são permitidos.
  - No cartão, o valor líquido entra na conta no próprio dia; o prazo de depósito da maquininha não é acompanhado.
- **Nota:** cada recebimento nasce com nota pendente. A secretária informa o número e a data quando emite a nota no Emissor Nacional.
- **Lista de notas pendentes:** mostra os dados de quem paga, o valor e uma descrição pronta do serviço.
- **Estorno de recebimento:** exige motivo e fica no histórico. Se a nota já tinha sido emitida, o sistema avisa para cancelá-la no Emissor Nacional.

### Repasse

**Atendimentos de parceiro.** O repasse é calculado sobre cada pacote de sessões, avaliação em pacote ou sessão avulsa. O percentual usado é o do parceiro no momento da venda ou da marcação, e fica gravado. Sessões de sócia não geram repasse.

**Repasse acumulado.** Sempre que o sistema calcula o repasse até uma data, ele faz assim:

- **Pacote de sessões:** as sessões realizadas ou com falta cobrada que usaram o pacote são cobertas pelo dinheiro já recebido do pacote, na ordem em que aconteceram.
  - Cada sessão coberta, toda ou em parte, gera para o parceiro dela o percentual sobre a parte coberta do valor por sessão.
  - Se o pacote tiver sessões de mais de um profissional, cada uma vai para quem atendeu.
- **Sessão avulsa:** se realizada ou falta cobrada, gera o percentual sobre o menor valor entre o preço da sessão e o que já foi recebido da cobrança dela.
- **Avaliação em pacote:** gera zero até a entrega do laudo; depois, o percentual sobre tudo o que já foi recebido da avaliação.

**A liberar.** É o percentual sobre a parte realizada que ainda não foi paga pelo paciente.

**Repasse devido no mês.** É o acumulado até o fim do mês menos o acumulado até o fim do mês anterior. Contam as datas das sessões, dos recebimentos e do laudo.

**Fechamento de repasse.** Congela, para cada parceiro e cada atendimento, o acumulado até o fim do mês.
- O valor do fechamento seguinte é o acumulado atual menos o que já foi fechado.
- Assim, correções feitas depois, mesmo com datas de meses fechados, entram como ajuste no fechamento seguinte, e um mês fechado nunca muda.
- Antes do fechamento, o painel mostra o valor provisório.

**Pagamento do repasse.** Registra data, conta e valor do fechamento de cada parceiro.

**Extrato.** Traz:
- cada atendimento: data, paciente, serviço, valor-base, percentual e repasse;
- os ajustes;
- o total;
- o que está a liberar.

Sai em PDF, com uma mensagem-resumo pronta no WhatsApp.

### Despesas, contas e caixa

- **Despesa:** descrição, categoria, valor, vencimento, mês de competência (padrão: o mês do vencimento), fornecedor (texto opcional) e linha de receita (opcional; se preenchida, é custo direto daquela linha). Quando for paga, ganha data e conta.
- **Despesa recorrente:** valor, dia do mês, categoria, linha e início. É criada todo mês até ser encerrada.
- **Guia de imposto:** é uma despesa da categoria "Impostos" que exige o mês de competência. Ela não entra no resultado, porque o resultado já usa o imposto estimado, mas entra no caixa.
- **Contas:** banco(s), caixa da secretária e maquininha(s), cada uma com saldo inicial na data de início. O saldo é o inicial, mais os recebimentos líquidos, menos os pagamentos, mais ou menos as transferências.
- **Pagamentos** são de quatro tipos: despesa, repasse, devolução e divisão de lucros.

### Apuração do mês

Contas feitas para um mês M:

- **Faturamento da linha** = soma do faturamento com competência M (devoluções contam negativo).
- **Imposto estimado da linha** = alíquota vigente em M × faturamento da linha. A alíquota tem data de vigência, e mudá-la não altera meses passados.
- **Repasse da linha** = repasse devido em M dos atendimentos daquela linha. Num mês já fechado é o valor do fechamento; num mês aberto, o valor provisório.
- **Taxas da linha** = soma das taxas dos recebimentos com data em M, cujas cobranças são daquela linha.
- **Despesas da linha** = despesas com competência M marcadas com a linha (exceto guias de imposto).
- **Lucro da linha** = faturamento − imposto estimado − repasse − taxas − despesas da linha.
- **Despesas gerais** = despesas com competência M sem linha (exceto guias de imposto).
- **Resultado** = soma do lucro das linhas − despesas gerais.
- **Caixa do mês** = recebimentos líquidos com data em M − pagamentos com data em M de despesas, repasses e devoluções. A divisão de lucros e as transferências não entram.
- **Lucro a dividir** = recebimentos líquidos em M − despesas pagas em M (exceto guias) − devoluções pagas em M − repasse devido em M − imposto estimado de M − diferença das guias pagas em M (valor da guia menos o imposto estimado da competência dela).
  - Na prática, é o caixa do mês trocando o repasse e a guia pagos no mês (que são de meses anteriores) pelo repasse e pelo imposto do próprio mês. A tela mostra essa conta passo a passo.
- **Fundo de reserva** = percentual configurado × lucro a dividir, quando o lucro a dividir é positivo.
- **Divisão de lucros** = (lucro a dividir − fundo de reserva) × participação de cada sócia (padrão de 25% cada; a soma tem de dar 100%). Se o lucro a dividir não for positivo, não há o que dividir.
- **Pagamentos às sócias:** cada sócia tem o seu pagamento registrado, com data e conta.

**Mês de exemplo.** Este é o mês de referência aprovado pelas sócias, que vira teste de aceitação. Imposto de 6% e fundo de reserva de 10%.

| # | O que aconteceu | Faturamento | Recebido (líquido) |
|---|---|---:|---:|
| 1 | Ana, paciente da sócia Carla: pacote de 4 sessões no Pix, as 4 realizadas | 800 | 800 |
| 2 | Bruno, paciente do parceiro Diego: pacote de 4 sessões no cartão (taxa de R$ 24). Teve 3 sessões realizadas e 1 falta abonada | 800 | 776 |
| 3 | Elisa, paciente do Diego: 1 sessão avulsa realizada e não paga | 200 | 0 |
| 4 | Fábio, avaliação com a parceira Gabriela: R$ 2.500 em 2 parcelas, só a 1ª paga, laudo não entregue | 2.500 | 1.250 |
| 5 | Locatária Helena: aluguel fixo de um turno às terças, 4 no mês | 1.200 | 1.200 |
| 6 | Diária do auditório | 600 | 600 |
| 7 | Supervisão (lançamento simples) | 400 | 400 |
| | **Total** | **6.500** | **5.026** |

Despesas pagas no mês:
- gerais: aluguel do imóvel (1.500), contador (500) e anúncios (300);
- custo direto da avaliação: folhas de resposta (156).

Valores esperados:

| | Valor (R$) |
|---|---:|
| Lucro por linha: Psicoterapia (1.800 − 108 − 180 − 24) | 1.488 |
| Lucro por linha: Avaliação (2.500 − 150 − 156) | 2.194 |
| Lucro por linha: Aluguel de salas e auditório (1.800 − 108) | 1.692 |
| Lucro por linha: Supervisão (400 − 24) | 376 |
| Despesas gerais | −2.300 |
| Resultado | 3.450 |
| Caixa do mês (5.026 − 2.456) | 2.570 |
| Lucro a dividir (2.570 − 180 − 390) | 2.000 |
| Fundo de reserva | 200 |
| Divisão de lucros para cada sócia | 450 |

Outros valores esperados:
- **Repasse:** Diego R$ 180 devidos e R$ 60 a liberar (sessão da Elisa); Gabriela R$ 0.
- **Painel:** inadimplência de R$ 200; R$ 1.250 a receber; crédito de 1 sessão (R$ 200); 6 notas pendentes.
- **Produção:** Carla R$ 800, Diego R$ 1.000, Gabriela R$ 2.500.

### Indicadores

- **Sessões:** realizadas, faltas cobradas, faltas abonadas e canceladas no mês. Taxa de faltas = faltas (cobradas e abonadas) ÷ (realizadas + faltas).
- **Ocupação das salas de aluguel e do auditório:** horas ocupadas por sessões não canceladas, aluguéis e bloqueios ÷ (horas de funcionamento × número de salas) no mês, mostrada no total e por sala.
- **Ocupação da agenda de cada profissional:** horas de sessões não canceladas ÷ horas de atendimento cadastradas do profissional. Se ele não tiver horário cadastrado, usa o horário de funcionamento.
- **Pacientes:**
  - Novos: tiveram a 1ª sessão realizada no mês.
  - Ativos no fim do mês: tiveram sessão realizada nos últimos 30 dias ou têm sessão marcada.
  - Pararam de vir: estavam ativos no fim do mês anterior e não estão ativos no fim deste mês.
- **Contatos:** novos no mês por origem, convertidos por origem, "não fechou" por motivo e taxa de conversão.
- **Lista de espera:** pessoas aguardando e tempo médio de espera.
- **Operação:** a receber nos próximos 30 dias, inadimplência, a pagar nos próximos 30 dias, saldo de cada conta, crédito de sessões (quantidade e valor) e notas pendentes.
- **Só para as sócias:** faturamento, resultado, lucro por linha, produção por profissional, caixa do mês, lucro a dividir e divisão de lucros.
- **Produção:** o pacote conta para o profissional do pacote, no mês da venda; a sessão avulsa, para quem atendeu; a avaliação, para o profissional dela.
- **Comparação:** todos os indicadores mensais podem ser vistos nos últimos 12 meses.

### Contatos e lista de espera

- **Contato:** é uma pessoa com data, origem, telefone, o que procura (terapia infantil, adolescente, adulto, casal ou família, avaliação neuropsicológica, avaliação infantil, outro) e situação. As situações são: novo, 1ª consulta agendada, virou paciente ou não fechou. "Não fechou" exige um motivo: preço, horário, desistiu ou outro.
- **Avanço automático:**
  - Agendar a 1ª consulta a partir do contato marca a sessão e muda a situação.
  - A 1ª sessão realizada transforma o contato em paciente, levando a origem junto.
- **Lista de espera:** cada entrada tem pessoa (contato ou paciente), o que precisa, preferência de dias e horários, tipo de profissional, data de entrada e situação (aguardando, atendido ou desistiu).
- **Alcance na Fase 1:** só pessoas que buscam atendimento. Interessados em cursos e aluguel ficam para a Fase 2.

### Aluguéis e bloqueios

- **Preços padrão** (ajustáveis):
  - hora: R$ 50;
  - turno: R$ 300;
  - diária do auditório: R$ 600, por 8 horas seguidas.
- **Turnos:** são os blocos configurados.
  - Segunda a sexta: 7h às 13h e 13h às 19h.
  - Sábado: 6h às 12h.
- **Aluguel avulso:** locatário, sala ou auditório, modalidade (hora, turno ou diária), data e horário. O valor é o preço da modalidade, que pode ser editado.
- **Aluguel por hora:** em horas inteiras, dentro do horário de funcionamento.
- **Aluguel fixo:** locatário, sala, dia da semana, modalidade e horário. A cobrança de cada mês é o número de datas no mês vezes o preço da modalidade, gerada no dia 1º.
- **Encerramento:** cancelar um aluguel, ou encerrar um aluguel fixo, libera a sala dali em diante. Datas já cobradas cujo aluguel foi cancelado têm a cobrança ajustada pela secretária.
- **Bloqueio:** sala ou auditório, período e motivo. Não gera cobrança, mas conta na ocupação.

### WhatsApp

Os botões abrem o WhatsApp com o texto pronto, usando o número da pessoa, e nada é enviado automaticamente. Os textos-padrão são editáveis nas configurações e usam o nome da clínica, o endereço e a chave Pix. Há quatro mensagens:
- lembrete ou confirmação da sessão, para o paciente;
- agenda do dia ou da semana, para o parceiro;
- resumo do extrato de repasse, para o parceiro (o PDF é baixado e anexado à mão);
- cobrança vencida, para quem paga.

### Começo do uso, importação e relatórios

- **Data de início:** é escolhida pelas sócias (por exemplo, o dia 1º de um mês). Os saldos iniciais entram como:
  - saldo de cada conta;
  - cobranças em aberto, sem faturamento no período;
  - despesas em aberto;
  - pacotes já pagos com o crédito restante: sem faturamento, com o valor tratado como já recebido, para que as sessões de parceiro gerem repasse.
- **Histórico:** o histórico anterior à data de início não é importado.
- **Importação de pacientes:** planilha-modelo (Excel) com nome, telefone, e-mail, CPF, data de nascimento, dados do responsável financeiro e profissional de referência.
  - O sistema mostra uma prévia com erros e possíveis duplicados (mesmo CPF, ou mesmo nome e telefone) antes de confirmar.
  - A planilha nunca é guardada no repositório.
- **Relatório do contador** (Excel, por mês), com as abas:
  - recebimentos: data, quem pagou, CPF ou CNPJ, descrição, linha, valor bruto, taxa, líquido, forma e nota;
  - repasses fechados por parceiro: CPF, nota ou RPA, e valor;
  - despesas pagas;
  - resumo: faturamento por linha e imposto estimado.
- **Exportar tudo:** gera um Excel com todos os cadastros e movimentos, só para as sócias.

### Histórico de alterações

Toda criação, alteração ou exclusão em sessões, aluguéis, bloqueios, pacotes, cobranças, recebimentos, despesas, repasses, divisões de lucros, configurações e usuários grava quem fez, quando, o quê, e os valores antes e depois. O histórico não pode ser editado.

### Segurança e LGPD

- Só quem tem login entra, e cada pessoa tem o seu. A senha é guardada com hash forte, e todo acesso é por conexão segura (https).
- Os dados ficam em São Paulo, criptografados pelo fornecedor, com cópia de segurança contínua e restauração aos últimos dias, conforme o plano do Neon.
- O sistema não guarda nada clínico (ADR-0001). Do paciente, guarda só o necessário para agenda, cobrança e nota.
- Testes, sementes e o site de demonstração usam somente dados inventados.

### Modelo de dados (conceitual)

**Cadastros:**
- pessoa, com papéis;
- profissional, com horário de atendimento;
- usuário, com perfil;
- serviço, sala, horário de funcionamento e turno;
- convênio de desconto e valor combinado.

**Agenda:**
- sessão e horário fixo;
- aluguel e aluguel fixo;
- bloqueio;
- ocupação: tabela técnica única que garante a ausência de choques de sala e de profissional.

**Venda e cobrança:**
- pacote (de sessões ou de avaliação), com uso de crédito por sessão;
- faturamento (registro de receita por linha e mês);
- cobrança (parcelas);
- recebimento, com nota;
- devolução.

**Repasse:**
- fechamento de repasse, com o acumulado congelado por atendimento e o pagamento.

**Despesas e contas:**
- despesa e despesa recorrente;
- conta, transferência e pagamento.

**Apuração:**
- alíquota com vigência;
- regra de divisão e fundo de reserva com vigência;
- divisão de lucros do mês e pagamentos às sócias.

**Contatos:**
- contato e entrada da lista de espera.

**Histórico:**
- registro de alterações.

### Valores iniciais na instalação

- **Salas:** 4 salas de sócia (uma por sócia), 4 salas de aluguel e 1 auditório.
- **Funcionamento e turnos:** segunda a sexta das 7h às 19h, com turnos de 7h às 13h e de 13h às 19h; sábado das 6h às 12h, com um turno.
- **Serviços:**
  - "Sessão de psicoterapia" (R$ 200, 50 min);
  - "Avaliação — pacote completo" (R$ 2.500);
  - "Avaliação — sessão avulsa" (R$ 350).
  - As sócias acrescentam os demais.
- **Repasse padrão:** 30%. Regra das 24 horas. Horizonte do horário fixo: 12 semanas. Paciente inativo após 30 dias.
- **Preços de aluguel:** R$ 50 a hora, R$ 300 o turno e R$ 600 a diária do auditório. Vencimento do aluguel fixo no dia 5.
- **Divisão de lucros:** 25% para cada sócia. Fundo de reserva de 0%, ajustável.
- **Categorias de despesa:** aluguel e condomínio; energia, água, internet e telefone; pessoal da secretária; pró-labore; impostos; marketing e anúncios; material e testes; limpeza e copa; manutenção; contador e serviços; sistemas e assinaturas; tarifas bancárias; custos de cursos; outras.
- **Origens:** Instagram, Google, site, indicação de paciente, indicação de profissional, CAACE e outra.
- **Convênio de desconto:** CAACE.
- **Informados pelas sócias na instalação:** alíquota estimada (vinda do contador), percentual do desconto CAACE, taxas da maquininha por forma, contas, chave Pix e endereço.

## Decisões de teste

- **Um bom teste** confere o comportamento visível pela interface do sistema (os casos de uso e as consultas), nunca detalhes internos. Os nomes dos testes usam os termos do glossário.
- **Ponto de teste principal: a camada de casos de uso**, rodando contra um PostgreSQL real em memória (PGlite), com as mesmas migrações de produção, inclusive a restrição de exclusão de choques. Esse é o único ponto de entrada dos testes de regra.
- **Testes de aceitação** obrigatórios:
  - O mês de exemplo acima, montado só com casos de uso, conferindo cada valor esperado.
  - Dois meses seguidos: repasse do mês 1 pago no mês 2 e guia real diferente da estimativa, conferindo o lucro a dividir dos dois meses.
  - Repasse:
    - pacote pago em parte;
    - sessões do mesmo pacote com profissionais diferentes;
    - avaliação com laudo e parcela recebida depois;
    - correção depois do fechamento virando ajuste no mês seguinte.
  - Agenda:
    - choque de sala e de profissional, inclusive duas marcações simultâneas;
    - horário fixo com data em choque;
    - regra das 24 horas, abono, desfazer, remarcar e cancelar.
  - Crédito:
    - uso do crédito e crédito esgotado virando avulsa;
    - inclusão de avulsas no pacote novo;
    - devolução.
  - Permissões: a secretária é barrada em cada caso de uso e consulta exclusivos das sócias.
  - Importação: planilha válida, inválida e com duplicados. Saldos iniciais.
  - Rotina diária rodando duas vezes sem duplicar sessões, cobranças nem despesas.
- **Funções puras** de datas, turnos e arredondamento podem ter testes unitários próprios quando isso deixar o erro mais claro.
- **Testes de ponta a ponta** (Playwright, no navegador) cobrem poucos fluxos principais pela tela:
  - login;
  - marcar e registrar sessão;
  - vender pacote;
  - registrar recebimento;
  - fechar repasse;
  - ver o painel.
- **Integração contínua no GitHub:** verificação de estilo, checagem de tipos, testes e build a cada envio. A publicação em produção só acontece com tudo verde.
- **Exemplos anteriores:** nenhum; o repositório começa do zero.

## Fora do escopo

- Prontuário e qualquer conteúdo clínico (ADR-0001).
- Planos de saúde e faturamento de convênio médico.
- Acesso de parceiros, pacientes ou contador; portal do paciente; agendamento online.
- Envio automático de WhatsApp (Fase 3).
- Emissão de nota fiscal pelo sistema (Fase 3, com o Emissor Nacional).
- Importação de extrato bancário, conciliação e prazo de depósito da maquininha (Fase 3).
- Gateway de pagamento e cobrança automática por Pix ou boleto.
- Módulos próprios de aluguel de testes com estoque, cursos e turmas, compras e estoque, e custo por paciente de cada canal de marketing (Fase 2). Na Fase 1, essas receitas entram como lançamento simples.
- Contatos de interessados em cursos ou aluguel (Fase 2).
- Divisão de despesas gerais entre linhas de receita.
- Cálculo de retenções de RPA (INSS, IR, ISS), que fica com o contador.
- Cobrança mensal no fim do mês (pós-paga); só existem pacote e avulso.
- Mais de uma unidade, aplicativo de celular nativo, verificação em duas etapas e recuperação de senha por e-mail.
- Demonstrativos contábeis oficiais e folha de pagamento.

## Notas adicionais

- **Custo mensal estimado:** R$ 135 a 160. São US$ 20 da Vercel Pro mais cerca de US$ 5 a 10 do Neon, com o dólar a R$ 5,12 e IOF. O valor exato é confirmado na criação das contas.
- **O que só as sócias podem fazer:**
  - criar as contas na Vercel e no Neon no nome da clínica, com cartão da empresa e verificação em duas etapas;
  - ligar a Vercel ao repositório;
  - opcionalmente, pedir o subdomínio sistema.naappsicologia.com.br a quem cuida do site.
  - Um passo a passo será entregue na hora certa.
- **Antes de assinar o Neon:** conferir a janela de restauração do plano, que deve ser de pelo menos 7 dias. Se for menor, escolher o plano que a garanta.
- **Ordem de construção:** a Fase 1 será construída em entregas que as sócias testam num site de demonstração, com dados inventados, antes da versão oficial. A ordem fica no plano de implementação.
- **Próximas fases:**
  - **Fase 2:** aluguel de testes com estoque de folhas de resposta, cursos e turmas, compras e estoque, e custo por paciente de cada canal.
  - **Fase 3:** WhatsApp automático, emissão de nota pelo Emissor Nacional e importação de extrato bancário.
  - Cada fase terá a sua rodada de perguntas antes de começar.
