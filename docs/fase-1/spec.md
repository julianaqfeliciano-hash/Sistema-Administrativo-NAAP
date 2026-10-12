# Fase 1 — Sistema de gestão do NAAP

Status: rascunho revisado em 12/10/2026. Aguarda as respostas da rodada 4 (Q36 a Q46) e a revisão das sócias. Onde a rodada 4 ainda não respondeu, o texto usa a resposta recomendada e indica a pergunta entre parênteses.

Os termos do negócio seguem o glossário do projeto. Decisões de arquitetura: ADR-0001 (sem conteúdo clínico), ADR-0002 (a agenda é a origem das cobranças e dos repasses) e ADR-0003 (tecnologia e hospedagem).

**Como ler:** as partes "Arquitetura", "Segurança, LGPD e operação técnica" e "Modelo de dados", e a seção "Decisões de teste", são técnicas e servem a quem vai programar. As regras do negócio estão nas demais partes.

## Problema

O NAAP tem sete linhas de receita, quatro sócias e parceiros, mas não tem nenhum sistema de gestão. Hoje a secretária marca as sessões e guarda os pacientes numa planilha de Excel. Por isso as sócias:

1. não sabem o lucro real do mês nem quanto rende cada linha de receita;
2. calculam à mão o repasse dos parceiros;
3. não têm controle confiável de quem pagou, de quem está devendo e do caixa.

Também não sabem de onde vêm os pacientes novos, quantas pessoas ficam sem horário, quanto as salas de aluguel e o auditório ficam ocupados, nem quais notas fiscais faltam emitir.

## Solução

Um sistema web interno, com login, que funciona no computador e no celular, usado pelas quatro **sócias** e pela **secretária**. Parceiros, pacientes e o contador não acessam o sistema.

- A **agenda** do sistema passa a ser a única agenda do NAAP. Ela reúne **sessões**, **aluguéis** de sala e de auditório e **bloqueios** do auditório, sem choque de sala nem de profissional.
- Cada sessão realizada (ou **falta cobrada**) usa o **crédito de sessões** do **pacote** ou gera uma **cobrança** de **sessão avulsa**, e entra no **repasse** do **parceiro** que atendeu.
- Todas as receitas entram no sistema: pacotes, sessões avulsas, aluguéis e **lançamentos simples** para supervisão, aluguel de testes, cursos e empresas.
- A secretária registra **recebimentos** (com taxa da maquininha e **valor líquido**), controla as **notas**, lança despesas e paga o repasse. O extrato do parceiro sai em PDF e com mensagem pronta no WhatsApp.
- O painel responde, em um clique, às perguntas da operação. As sócias veem também **faturamento**, **resultado**, lucro por linha e por profissional, **produção**, **caixa do mês**, **lucro a dividir** e **divisão de lucros**.
- Os **contatos** novos são registrados com **origem** até virarem pacientes ou não fecharem. Quem não encontra horário fica na **lista de espera**.
- O sistema não guarda nenhum conteúdo clínico.

## Histórias de usuário

### Acesso e segurança

1. Como sócia, quero criar logins, desativá-los e redefinir senhas das sócias e da secretária, para controlar quem acessa o sistema sem depender de e-mail automático.
2. Como usuária, quero entrar com e-mail e senha, ter o botão Sair sempre visível e ser desconectada depois de 1 hora sem uso (Q45), para proteger os dados dos pacientes num computador compartilhado.
3. Como sócia, quero que a secretária não consiga ver nem fazer o que é exclusivo das sócias, mesmo digitando o endereço da página, para manter essas informações só conosco. Isso inclui:
   - faturamento, resultado, lucro por linha e por profissional, produção, caixa do mês, lucro a dividir e divisão de lucros;
   - saldo das contas de banco e fechamento de repasse (Q36);
   - configurações, usuários, saldos iniciais, histórico de alterações, exportar tudo e anonimizar pessoa.
4. Como sócia, quero ver o histórico de quem criou, alterou ou apagou cada registro de agenda, dinheiro, cadastro e configuração, e quando, para confiar nos números.
5. Como sócia, quero exportar todos os dados para Excel, para guardar uma cópia por conta própria.
6. Como sócia, quero anonimizar uma pessoa ou um contato a pedido dele, para cumprir a LGPD sem apagar os números do NAAP.

### Cadastros

7. Como secretária, quero cadastrar uma **pessoa** uma única vez e dar a ela um ou mais papéis (**paciente**, **responsável financeiro**, **parceiro**, **locatário**, **aluno**), para não duplicar pessoas.
8. Como secretária, quero indicar o responsável financeiro de cada paciente (o próprio paciente, um familiar ou outra pessoa), para que a cobrança e a nota saiam no nome de quem paga.
9. Como secretária, quero registrar o **convênio de desconto** e o valor combinado de um paciente, para que os preços já saiam certos.
10. Como secretária, quero buscar pessoas por nome, telefone ou CPF, para achar o cadastro rápido.
11. Como secretária, quero ver na ficha do paciente os **horários fixos**, o crédito de sessões, as cobranças em aberto, os recebimentos e as próximas sessões, para atender sem procurar em vários lugares.
12. Como sócia, quero definir o percentual de repasse de cada parceiro (padrão de 30%), inclusive diferente por serviço, e se ele manda nota ou RPA, para o repasse sair calculado.
13. Como sócia, quero cadastrar os **serviços** com linha de receita, preço de tabela, duração e tipo, para que agenda e cobrança usem a tabela.
14. Como sócia, quero cadastrar as **salas** (de sócia e de aluguel), o **auditório**, o horário de funcionamento, os **turnos** e os **dias sem funcionamento** (feriados e recessos), para que agenda, aluguel e ocupação reflitam a clínica.
15. Como sócia, quero configurar:
    - preços de aluguel, alíquota estimada do imposto, regra de divisão de lucros e **fundo de reserva**;
    - **contas** e taxas da maquininha por forma de recebimento;
    - categorias de despesa, origens, convênios de desconto, chave Pix e endereço;

    para ajustar o sistema sem programador.

### Agenda

16. Como secretária, quero ver a grade do dia e da semana por sala (com uma coluna "online") e por profissional, para saber o que está ocupado.
17. Como secretária, quero marcar uma sessão escolhendo paciente, profissional, serviço (ou a avaliação do paciente), sala ou online, data e hora, para organizar as sessões.
18. Como secretária, quero que o sistema não deixe usar a mesma sala, nem marcar o mesmo profissional, duas vezes no mesmo horário, mesmo que duas pessoas marquem ao mesmo tempo, para evitar choques.
19. Como secretária, quero criar um horário fixo semanal para um paciente, para que as sessões apareçam sozinhas nas próximas semanas até eu encerrar.
20. Como secretária, quero alterar um horário fixo (dia, hora, sala, profissional ou serviço) a partir de uma data, para acompanhar as mudanças sem refazer a agenda à mão.
21. Como secretária, quero encerrar um horário fixo numa data, vendo antes o que será apagado e podendo manter alguma sessão, para não perder uma reposição já combinada.
22. Como secretária, quero ver os "conflitos a resolver" quando uma data de horário fixo ou de aluguel fixo não puder ser criada, para resolver antes do dia.
23. Como secretária, quero remarcar uma sessão para outro dia, hora, sala ou profissional, para acomodar pedidos.
24. Como secretária, quero marcar a sessão como realizada, para que crédito, cobrança e repasse se atualizem sozinhos.
25. Como secretária, quero registrar que o paciente não compareceu, para que a sessão vire falta cobrada.
26. Como secretária, quero registrar que o paciente desmarcou, informando quando ele avisou, para que o sistema aplique a regra das 24 horas.
27. Como secretária, quero abonar uma falta cobrada, para não cobrar quando houver motivo aceito.
28. Como secretária, quero cancelar uma sessão pela clínica (por exemplo, profissional doente), para liberar o horário sem cobrar o paciente.
29. Como secretária, quero desfazer o registro de realizada, falta ou cancelamento feito por engano, voltando a sessão para agendada, para corrigir sem bagunçar crédito, cobrança e repasse.
30. Como secretária, quero ver as sessões de dias passados que ainda estão como "agendada", para não esquecer de registrar o que aconteceu.
31. Como secretária, quero abrir o WhatsApp com o lembrete ou a confirmação já escritos, para avisar o paciente com um clique.
32. Como secretária, quero abrir o WhatsApp com a agenda do dia ou da semana de um parceiro, para ele saber suas sessões sem acessar o sistema.

### Aluguéis e bloqueios

33. Como secretária, quero registrar um aluguel avulso de sala de aluguel por hora ou por turno, ou do auditório por diária, para cobrar o locatário.
34. Como secretária, quero registrar um aluguel fixo semanal, para que as datas apareçam na agenda e a cobrança do mês seja gerada sozinha.
35. Como secretária, quero ajustar o valor de um aluguel, para casos negociados.
36. Como secretária, quero cancelar uma data de aluguel, avulso ou fixo, aplicando a regra de cobrança combinada, para liberar a sala sem errar a cobrança.
37. Como secretária, quero bloquear o auditório para uso do NAAP, por exemplo um curso, para que ninguém o alugue nesse horário.

### Pacotes, avaliações e cobranças

38. Como secretária, quero vender um pacote de sessões com quantidade, valor e parcelas, para registrar a venda paga adiantada.
39. Como secretária, quero que cada sessão realizada ou falta cobrada use uma sessão do crédito do mesmo serviço, e que a falta abonada não use, para o crédito ficar sempre certo.
40. Como secretária, quero ser avisada quando o crédito de um paciente que comprava pacote acabar, para vender um novo pacote.
41. Como secretária, quero que uma sessão feita sem crédito vire cobrança de sessão avulsa, para não perder cobrança.
42. Como secretária, quero vender uma **avaliação** como pacote completo, com parcelas, ou cobrar sessões avulsas de avaliação, para refletir as duas formas de venda.
43. Como secretária, quero registrar a entrega do laudo, e desfazê-la com motivo se errar, para concluir a avaliação e liberar o repasse do parceiro.
44. Como secretária, quero encerrar sem laudo uma avaliação interrompida, para cancelar as parcelas que não serão pagas e acertar o repasse.
45. Como secretária, quero fazer lançamentos simples (supervisão, aluguel de testes, cursos, avaliação psicossocial para empresas) na linha de receita certa, informando quem atendeu quando houver, para que todas as receitas estejam no sistema já na Fase 1.
46. Como secretária, quero registrar a **devolução** de crédito de sessões não usado, para que faturamento e caixa do mês mostrem o dinheiro devolvido.
47. Como secretária, quero ver todas as cobranças com vencimento, filtrando por pessoa, linha e situação, para cobrar em dia.
48. Como secretária, quero ver as cobranças vencidas e abrir o WhatsApp com uma única mensagem de cobrança por responsável financeiro (valores, vencimentos e chave Pix), para reduzir a **inadimplência**.

### Recebimentos e notas

49. Como secretária, quero registrar um recebimento (data, valor, forma, conta), com a taxa da maquininha calculada pela forma, para saber o valor líquido.
50. Como secretária, quero registrar mais de um recebimento para a mesma cobrança, para quando o responsável financeiro paga em partes.
51. Como secretária, quero que todo recebimento comece com nota pendente e eu possa informar o número e a data da nota emitida, para não esquecer nenhuma.
52. Como secretária, quero uma lista das notas pendentes com os dados de quem paga e uma descrição pronta, que inclui o nome do paciente quando ele não é quem paga, para emitir no Emissor Nacional rápido.
53. Como secretária, quero cancelar um recebimento lançado errado, com motivo, para corrigir os números com rastro no histórico.

### Repasse

54. Como secretária, quero ver, a qualquer momento do mês, o repasse provisório de cada parceiro e o que está **a liberar**, para responder aos parceiros.
55. Como sócia, quero fechar o repasse do mês, congelando o valor de cada parceiro, para aprovar o que será pago (Q36).
56. Como secretária, quero registrar o pagamento do repasse fechado de cada parceiro (data e conta), para o saldo da conta e o caixa do mês ficarem certos.
57. Como secretária, quero gerar o extrato do repasse em PDF e abrir o WhatsApp com o resumo, para o parceiro conferir.
58. Como parceiro, quero receber um extrato com uma linha por sessão e por avaliação (data, paciente, serviço, valor-base, percentual e repasse), os ajustes, o saldo a descontar e o que está a liberar, para conferir meu pagamento.
59. Como sócia, quero que mudanças feitas depois de um **fechamento de repasse** entrem como ajuste no fechamento seguinte, para nunca mexer num mês já pago. Exemplos: falta abonada depois do fechamento, parcela recebida depois, entrega de laudo.

### Despesas, contas e caixa

60. Como secretária, quero lançar despesas com categoria, valor, vencimento, mês de competência, fornecedor e, quando for **custo direto**, a linha de receita, para separar custos diretos de **despesas gerais**.
61. Como secretária, quero cadastrar despesas que se repetem todo mês, para que sejam criadas sozinhas até eu encerrar.
62. Como secretária, quero registrar o pagamento de uma despesa (data e conta), para o saldo da conta e o caixa do mês ficarem certos.
63. Como secretária, quero lançar a **guia do Simples** com o mês a que ela se refere, para que o caixa do mês mostre o valor pago e a diferença entre a guia e o imposto estimado daquele mês entre no lucro a dividir.
64. Como secretária, quero registrar **transferências** entre contas (como o depósito do dinheiro do caixa da secretária no banco, ou o repasse da maquininha para o banco), para os saldos baterem.
65. Como secretária, quero ver o saldo do caixa da secretária e da maquininha, para conferir com o dinheiro e com o aplicativo da maquininha (Q36).
66. Como secretária, quero ver as despesas vencidas e as que vencem nos próximos 30 dias, para não atrasar contas.

### Contatos e lista de espera

67. Como secretária, quero registrar cada contato com origem, telefone, o que procura e, quando for para outra pessoa (por exemplo, um filho), o nome dela, para medir de onde vêm os pacientes.
68. Como secretária, quero agendar a 1ª consulta a partir do contato, para não redigitar os dados.
69. Como secretária, quero que o contato passe a "virou paciente" quando a 1ª consulta for realizada, para a conversão ser contada sozinha.
70. Como secretária, quero marcar um contato como "não fechou", com o motivo, para entender por que perdemos pacientes.
71. Como secretária, quero colocar contatos e pacientes na lista de espera com o que precisam, os dias e horários de preferência e o profissional preferido, para chamar quando abrir vaga.
72. Como sócia, quero ver quantas pessoas estão na lista de espera e há quanto tempo esperam, para decidir se chamamos mais parceiros.

### Painel e relatórios

73. Como secretária, quero um painel **Hoje**, para acompanhar o dia a dia sem montar planilhas. Ele mostra:
    - a receber nos próximos 30 dias e inadimplência;
    - a pagar (vencidas e próximos 30 dias), com os repasses fechados ainda não pagos;
    - saldo do caixa da secretária e da maquininha;
    - crédito de sessões e notas pendentes;
    - repasse provisório e a liberar de cada parceiro;
    - lista de espera, pendências de registro e conflitos a resolver.
74. Como secretária, quero um painel de operação do mês, para acompanhar como a clínica está indo. Ele mostra:
    - sessões realizadas, faltas e cancelamentos;
    - ocupação das salas de aluguel e do auditório;
    - ocupação da agenda das sócias e horas de sessões dos parceiros;
    - pacientes novos, ativos e que pararam de vir;
    - contatos por origem e conversão;
    - entradas e saídas da lista de espera.
75. Como sócia, quero ver faturamento, resultado e lucro de cada linha de receita, mês a mês, e filtrar as linhas por profissional, para saber o que rende.
76. Como sócia, quero ver a produção e o lucro de cada profissional, para acompanhar quem gera receita.
77. Como sócia, quero ver o caixa do mês e o lucro a dividir com a conta explicada passo a passo, para dividir só o dinheiro que é nosso.
78. Como sócia, quero registrar a divisão de lucros do mês pela regra configurada, depois do fundo de reserva, e os pagamentos às sócias, para fechar o mês.
79. Como sócia, quero ver os indicadores dos últimos 12 meses lado a lado, para enxergar tendências.
80. Como secretária, quero gerar o relatório mensal do contador em Excel, para enviar todo mês. O relatório tem recebimentos com notas, repasses e despesas pagas; a aba de resumo só sai para as sócias.

### Começo do uso

81. Como secretária, quero baixar uma planilha-modelo de pacientes, preencher com os dados do Excel atual e importar vendo uma prévia antes de confirmar, para não digitar tudo de novo.
82. Como sócia, quero informar a **data de início** e os **saldos iniciais**, para começar com os números certos sem importar histórico. Os saldos iniciais são:
    - o saldo das contas;
    - as cobranças e despesas em aberto;
    - pacotes e avaliações em andamento;
    - o repasse ainda não pago.
83. Como secretária, quero cadastrar os horários fixos ativos no começo, para a agenda já nascer pronta.

## Decisões de implementação

### Perfis e permissões

| O quê | Sócia | Secretária |
|---|:-:|:-:|
| Agenda, aluguéis, bloqueios, pessoas, contatos, lista de espera | ✓ | ✓ |
| Pacotes, avaliações, lançamentos simples, cobranças, recebimentos, notas, devoluções | ✓ | ✓ |
| Despesas e seus pagamentos, transferências | ✓ | ✓ |
| Saldo do caixa da secretária e da maquininha | ✓ | ✓ |
| Saldo das contas de banco (Q36) | ✓ | — |
| Repasse: valor provisório, a liberar, extrato e pagamento do que foi fechado | ✓ | ✓ |
| Fechamento de repasse (Q36) | ✓ | — |
| Painel Hoje e painel de operação do mês | ✓ | ✓ |
| Relatório do contador sem a aba de resumo; importação de pacientes | ✓ | ✓ |
| Faturamento, resultado, lucro por linha e por profissional, produção, caixa do mês, lucro a dividir, divisão de lucros, aba de resumo do relatório do contador | ✓ | — |
| Configurações, usuários, saldos iniciais, histórico de alterações, exportar tudo, anonimizar pessoa | ✓ | — |

- **Configurações** são: serviços e preços de tabela; salas, auditório, funcionamento, turnos e dias sem funcionamento; preços de aluguel; convênios de desconto; percentuais de repasse; alíquota estimada; regra de divisão e fundo de reserva; contas e taxas da maquininha; categorias de despesa; origens; chave Pix e endereço.
- A secretária pode dar o papel de parceiro a uma pessoa, que nasce com o percentual padrão; só uma sócia muda percentuais.
- A secretária pode editar o preço de uma venda, o valor combinado de um paciente e o valor de um aluguel; cada edição fica no histórico (Q36).
- A permissão é conferida em cada caso de uso e em cada leitura de dados no servidor, não só escondendo botões. Parceiros, pacientes e o contador não têm login.

### Cadastros

- **Pessoa:** nome, CPF ou CNPJ, telefone com WhatsApp, e-mail, endereço (para a nota), data de nascimento (opcional) e observações administrativas.
  - A data de nascimento serve só para mostrar que o paciente é menor de idade e exigir um responsável financeiro diferente dele.
  - As observações têm no máximo 500 caracteres. Junto delas, e de todo campo de motivo, a tela mostra: "Só informações administrativas. Não escreva diagnóstico, queixa ou qualquer informação clínica."
  - Uma pessoa pode ter vários papéis.
- **Paciente:** tem um responsável financeiro (uma pessoa, que pode ser ele mesmo), um convênio de desconto opcional, valores combinados por serviço opcionais e a origem quando veio de um contato. Pacientes importados ou cadastrados no começo do uso ficam marcados como "paciente anterior à data de início".
- **Profissional:** é uma sócia ou um parceiro.
  - A sócia tem a sua sala de sócia.
  - O parceiro tem um percentual de repasse padrão (30%) e pode ter percentuais diferentes por serviço. Hoje todos usam 30% (Q27).
  - O parceiro tem também a indicação de se manda nota ou RPA, que serve só de informação para o contador.
- **Serviço:** nome, linha de receita, preço de tabela, duração padrão e tipo. Os tipos são:
  - "sessão" (psicoterapia e consultas, vendida em pacote ou como sessão avulsa);
  - "avaliação em pacote";
  - "sessão avulsa de avaliação".
- **Preço do paciente:** para um serviço, é o valor combinado do paciente, quando existe. Senão, é o preço de tabela menos o percentual do convênio de desconto dele. O convênio não se aplica sobre o valor combinado.

### Agenda

- **Choques:** ocupam sala e profissional as sessões agendadas, as realizadas e as faltas por "não compareceu". Também ocupam sala os aluguéis e os bloqueios. Duas ocupações da mesma sala, ou duas do mesmo profissional, não podem se sobrepor.
  - Sessões desmarcadas pelo paciente (canceladas ou falta cobrada pela regra das 24 horas) e sessões canceladas pela clínica liberam o horário.
  - O sistema garante isso no próprio banco de dados, e não só na tela: duas marcações ao mesmo tempo nunca passam.
- **Uso das salas** (Q31):
  - Cada sócia atende na própria sala ou online.
  - Parceiros e locatários usam as salas de aluguel; o auditório recebe aluguéis e bloqueios.
  - A sessão online não tem sala, a não ser que a secretária indique de qual sala o profissional atende; nesse caso, ocupa a sala.
  - Sessão presencial fora do horário de funcionamento é aceita, com aviso na tela.
- **Situações da sessão:** agendada, realizada, falta cobrada, falta abonada, cancelada pelo paciente ou cancelada pela clínica.
  - "Realizada" e "Não compareceu" só são aceitos a partir do horário de início da sessão.
  - "Não compareceu" vira falta cobrada. Aviso dado depois do início conta como não compareceu.
  - "Desmarcou" é o único jeito de registrar desistência do paciente e pede quando ele avisou. Aviso com 24 horas corridas ou mais antes do início (ajustável) vira cancelada pelo paciente; com menos, vira falta cobrada.
  - "Cancelar pela clínica" vira cancelada pela clínica, sem cobrança.
  - Uma falta cobrada pode ser abonada; a falta abonada não usa crédito e não gera cobrança.
  - "Desfazer" volta uma sessão registrada para agendada e reverte os efeitos dela no crédito, na cobrança e no repasse. Se a sessão liberou o horário, só volta se a sala e o profissional ainda estiverem livres.
  - Abonar ou desfazer uma sessão cuja cobrança de sessão avulsa já tem recebimento não é aceito: o sistema pede que o recebimento seja cancelado antes.
- **Remarcar** muda data, hora, sala ou profissional de uma sessão agendada, conferindo de novo os choques. Remarcar nunca gera falta cobrada, mesmo pedida pelo paciente com menos de 24 horas (Q42); quando a secretária quiser cobrar, registra "Desmarcou" e marca uma sessão nova.
- **Horário fixo:** paciente, profissional, serviço (ou avaliação), sala ou online, dia da semana, hora e data de início.
  - Gera as sessões das 12 semanas seguintes, nunca em datas passadas nem em dias sem funcionamento.
  - Cada sessão gerada guarda o horário fixo e a data prevista de origem, que não mudam se ela for remarcada ou cancelada. A rotina diária só cria uma data prevista que ainda não tem sessão, conflito ou dispensa registrados.
  - **Alterar** vale a partir de uma data escolhida. O horário fixo atual é encerrado na véspera e nasce um novo, ligado a ele. As sessões agendadas a partir dessa data são refeitas no novo horário; sessões já registradas não mudam.
  - **Encerrar** pede a data final, que pode ser passada ou futura. Antes de confirmar, a tela lista as sessões agendadas depois dela, que serão apagadas, e deixa manter alguma.
- **Conflitos a resolver:** uma data de horário fixo ou de aluguel fixo que cairia num choque ou num dia sem funcionamento não é criada e vira um conflito, guardado uma vez por data. A secretária tem três saídas:
  - marca em outra hora, sala ou profissional;
  - tenta de novo, se o choque saiu;
  - dispensa a data.

  O conflito sai da lista quando é resolvido ou quando a data passa.
- **Dias sem funcionamento:** são cadastrados pelas sócias. Neles, horários fixos e aluguéis fixos não geram datas. Cadastrar um dia que já tem sessões ou aluguéis lista esses itens para a secretária resolver.
- **Pendências de registro:** sessões de dias passados que ainda estão como agendadas.

### Aluguéis e bloqueios

- **Preços padrão** (ajustáveis):
  - hora: R$ 50;
  - turno: R$ 300;
  - diária do auditório: R$ 600, por 8 horas seguidas.
- **Turnos:** são os blocos configurados. Segunda a sexta: 7h às 13h e 13h às 19h. Sábado: 6h às 12h. O aluguel por turno ocupa exatamente um turno; seis horas em outro horário são aluguel por hora.
- **Aluguel por hora:** uma ou mais horas inteiras, começando em hora cheia, dentro do funcionamento.
- **Diária do auditório:** 8 horas seguidas dentro do funcionamento, começando na hora cheia escolhida. No sábado, o auditório é alugado pelo turno das 6h às 12h, com preço próprio informado pelas sócias (Q41).
- **Aluguel avulso:** locatário, sala ou auditório, modalidade (hora, turno ou diária), data, horário e valor. O valor é sugerido pela modalidade e pode ser editado.
- **Aluguel fixo:** locatário, sala ou auditório, dia da semana, modalidade, horário, valor por data e data de início.
  - O valor por data é sugerido pela modalidade (hora: horas × preço da hora; turno: preço do turno) e pode ser editado.
  - Como o horário fixo, gera as datas na agenda até 12 semanas à frente, com os mesmos conflitos a resolver.
  - A cobrança de cada mês é a soma do valor das datas do mês que não foram canceladas.
- **Cancelar uma data de aluguel** (avulso ou fixo) segue a regra das 24 horas (Q43). Com menos de 24 horas do início, a data continua cobrada e só libera a sala. Com 24 horas ou mais, a data deixa de ser cobrada:
  - se a cobrança ainda estiver aberta, o valor da data sai da cobrança e do faturamento do mês;
  - se já estiver paga, o valor é devolvido ao locatário como devolução.
- **Encerrar um aluguel fixo** pede a data final; as datas posteriores são apagadas e não entram na cobrança.
- **Bloqueio:** auditório, período e motivo. Não gera cobrança e não entra na ocupação, que mostra as horas bloqueadas à parte.
- **Parceiro que também aluga sala:** a cobrança do aluguel e o repasse são separados (Q44). O aluguel dele conta como ocupação do profissional no choque de agenda.

### Pacotes e crédito de sessões

- **Pacote de sessões:** paciente, responsável financeiro, serviço (do tipo "sessão"), profissional, quantidade de sessões, valor total, data da venda e parcelas (uma ou mais, com vencimentos). O valor total sugerido é o preço do paciente vezes a quantidade.
- **Valor por sessão:** o valor por sessão do pacote não é gravado arredondado.
  - A base acumulada das primeiras k sessões é arredondar(valor total × k ÷ quantidade), e a base de cada sessão é a diferença entre duas bases acumuladas. Exemplo: R$ 1.000 em 3 sessões dá 333,33 / 333,34 / 333,33.
  - O crédito restante, em reais, é o valor total menos a base acumulada das sessões usadas.
- **Uso do crédito:** uma sessão de serviço do tipo "sessão", ao virar realizada ou falta cobrada, usa uma sessão do pacote mais antigo do paciente, do mesmo serviço, que ainda tem crédito. Vale mesmo que o profissional seja outro.
  - Falta abonada e cancelamentos não usam crédito; desfazer devolve.
  - O crédito não vence.
- **Sem crédito:** a sessão vira sessão avulsa. Gera faturamento e cobrança no preço do paciente, vencendo no dia da sessão.
  - Se o paciente já teve pacote, o sistema avisa "crédito esgotado"; quem sempre pagou avulso não recebe o aviso.
  - Sessões avulsas continuam avulsas mesmo depois da venda de um novo pacote.
- **Devolução de crédito:** retira sessões não usadas do pacote.
  - O faturamento negativo é a base das sessões devolvidas, no mês da devolução.
  - Se o pacote tiver parcelas em aberto, a devolução primeiro cancela ou reduz essas parcelas, da mais nova para a mais antiga.
  - O pagamento sugerido ao responsável financeiro é só o que sobrar: o recebido menos a base das sessões usadas. Ele pode ser editado.
  - Exemplo: pacote 4 × 200, com 400 pagos, 1 sessão usada e 3 devolvidas. A 2ª parcela é cancelada, o pagamento é de 200 e o faturamento é de −600.

### Avaliação

- **Avaliação em pacote:** paciente, responsável financeiro, profissional, valor total, data da venda e parcelas.
- **Sessões da avaliação:** ao marcar uma sessão para um paciente com avaliação sem laudo, a secretária escolhe a avaliação no lugar do serviço. A sessão fica ligada a ela e usa a duração padrão do serviço de avaliação.
  - Essas sessões não usam crédito nem geram cobrança.
  - "Não compareceu" vira falta cobrada só para os indicadores.
  - As sessões ligadas podem ser de outro profissional, com aviso na tela. O repasse e a produção da avaliação vão inteiros para o profissional da avaliação.
- **Laudo entregue:** a secretária registra a data. Pode ser desfeito com motivo; se o repasse do mês já foi fechado, o desfazer vira ajuste negativo no fechamento seguinte. Depois do laudo, nenhuma sessão nova se liga à avaliação.
- **Encerrada sem laudo** (paciente desistiu), com data e motivo (Q40):
  - as parcelas em aberto são canceladas, e o valor delas sai como faturamento negativo no mês do encerramento;
  - o repasse do profissional é o percentual sobre o que já foi recebido;
  - se for preciso devolver dinheiro, a secretária registra uma devolução.
- **Avaliação vendida por sessão:** usa o serviço "Avaliação — sessão avulsa" (R$ 350). Cada sessão é cobrada como sessão avulsa e gera repasse como qualquer sessão avulsa (Q39).

### Faturamento e cobranças

**Faturamento** (em que mês a receita conta):

| Origem | Mês em que conta | Cobranças geradas | Vencimento |
|---|---|---|---|
| Pacote de sessões ou avaliação em pacote | mês da venda, pelo valor total (Q30) | uma por parcela | definido na venda (padrão: data da venda) |
| Sessão avulsa (inclusive falta cobrada sem crédito) | mês da sessão | uma | dia da sessão |
| Aluguel avulso | mês do uso | uma | dia do uso |
| Aluguel fixo | mês das datas | uma por mês | dia 5 do mês, ou a data de criação se for depois (ajustável) |
| Lançamento simples | mês informado | uma | data informada |
| Devolução, cancelamento de parcelas, data de aluguel cancelada no prazo | mês em que acontece (valor negativo) | — | — |

- **Lançamento simples:** linha de receita, pessoa que paga (cadastrada, com CPF ou CNPJ e endereço, para a nota), descrição, valor, mês de competência, vencimento e, opcionalmente, o profissional que atendeu. Não gera repasse.
- **Quem é cobrado:** o responsável financeiro do paciente, o locatário, ou a pessoa ou empresa informada no lançamento simples.
- **Situação da cobrança:** aberta, paga (quando a soma dos recebimentos chega ao valor) ou cancelada.
  - **Inadimplência** é a soma do que falta receber das cobranças abertas com vencimento antes de hoje.
  - **A receber nos próximos 30 dias** é a soma do que falta receber das cobranças abertas que vencem de hoje até 30 dias à frente.

### Recebimentos e notas

- **Recebimento:** cobrança, data, valor bruto, forma (Pix, dinheiro, transferência, cartão de débito ou de crédito), conta, taxa e valor líquido.
  - A conta é sugerida pela forma: Pix e transferência vão para o banco, dinheiro para o caixa da secretária, cartão para a maquininha.
  - A taxa é o percentual configurado para a forma, e pode ser editada.
  - Uma cobrança pode ter vários recebimentos.
  - No cartão, o valor líquido entra na conta da maquininha no próprio dia. Quando a maquininha deposita no banco, a secretária registra uma transferência.
- **Nota:** cada recebimento nasce com nota pendente, inclusive cada parcela paga de um pacote ou avaliação (Q38). A secretária informa o número e a data quando emite a nota no Emissor Nacional.
- **Lista de notas pendentes:** mostra os dados de quem paga, o valor e uma descrição pronta com o serviço e as datas ou o período.
  - Quando o paciente não é o responsável financeiro, a descrição inclui "Paciente: nome, CPF".
  - Nada clínico entra na descrição.
- **Cancelamento de recebimento:** exige motivo, tem data própria (a do registro) e fica no histórico. A cobrança volta a ficar aberta pelo valor cancelado.
  - Em todos os cálculos por data, o valor cancelado sai na data do cancelamento, e não na data original: recebimentos líquidos, taxas, saldo da conta e dinheiro recebido usado no repasse.
  - Se a nota já tinha sido emitida, o sistema avisa para cancelá-la no Emissor Nacional.

### Repasse

**Base do repasse.** O repasse é calculado sobre:
- cada sessão de parceiro, de pacote ou avulsa;
- cada avaliação em pacote de parceiro.

Sessões de sócia não geram repasse, mesmo usando um pacote vendido com um parceiro.

**Percentual.**
- Sessões: é gravado em cada sessão quando ela vira realizada ou falta cobrada. É o percentual do parceiro que atendeu, para aquele serviço.
- Avaliação em pacote: é o do parceiro da avaliação na data da venda.

**Recebido.** É sempre o valor bruto dos recebimentos, antes da taxa da maquininha, menos os cancelados. A taxa fica toda com o NAAP: uma sessão de R$ 200 paga no cartão rende R$ 60 ao parceiro.

**Repasse acumulado até uma data D:**
- **Pacote de sessões:** o recebido do pacote até D cobre as sessões realizadas ou com falta cobrada que usaram o pacote, pela ordem de início.
  - A sessão de sócia também consome essa cobertura, sem gerar repasse.
  - Cada sessão de parceiro gera o percentual dela sobre a parte coberta da sua base.
  - O acumulado de cada parceiro no pacote é arredondado ao centavo.
- **Sessão avulsa:** se realizada ou falta cobrada, gera o percentual sobre o menor valor entre o preço da sessão e o recebido da cobrança dela.
- **Avaliação em pacote:**
  - zero até a entrega do laudo ou o encerramento sem laudo;
  - depois, o percentual sobre o recebido da avaliação.

**A liberar.** É o percentual sobre a parte realizada das sessões que o responsável financeiro ainda não pagou. Na avaliação, é zero antes do laudo; depois dele, é o percentual sobre as parcelas ainda não recebidas.

**Repasse devido no mês M** = acumulado até o fim de M, calculado com os dados de hoje, menos tudo o que já foi fechado em meses anteriores.
- Antes do fechamento de M, esse é o valor provisório.
- Correções com datas de meses já fechados entram, assim, como ajuste no primeiro mês ainda não fechado.
- É esse valor que entra no resultado e no lucro a dividir.

**Fechamento de repasse.**
- Só pode ser feito depois que o mês acabou, e os meses são fechados em ordem.
- Congela o repasse devido de cada parceiro no mês, com o detalhe por sessão e por avaliação; um mês fechado nunca muda.
- É feito por uma sócia (Q36).
- Se o valor de um parceiro der negativo (ajustes maiores que o devido), não há pagamento naquele mês. O valor fica como saldo a descontar no fechamento seguinte e aparece no extrato.

**Pagamento do repasse.** Registra data, conta e valor do fechamento de cada parceiro.

**Extrato.** Traz:
- uma linha por sessão: data, paciente, serviço, valor-base, percentual e repasse;
- uma linha por avaliação: data do laudo, paciente, valor recebido, percentual e repasse;
- os ajustes de meses anteriores;
- o saldo a descontar, se houver;
- o total;
- o que está a liberar.

Sai em PDF, com uma mensagem-resumo pronta no WhatsApp.

### Despesas, contas e caixa

- **Despesa:** descrição, categoria, valor, vencimento, mês de competência (padrão: o mês do vencimento), fornecedor (texto opcional) e linha de receita (opcional; se preenchida, é custo direto daquela linha). Quando for paga, ganha data e conta.
- **Despesa recorrente:**
  - Campos: descrição, fornecedor (opcional), valor, dia do vencimento, categoria, linha (opcional), início e fim (opcional).
  - Gera a despesa de cada mês, inclusive a do mês corrente ao ser criada, se o dia ainda não passou.
  - Num mês sem aquele dia, vence no último dia do mês.
  - O valor de cada mês gerado pode ser editado sem mudar a recorrência.
- **Guia do Simples:** é uma despesa marcada como "Guia do Simples (DAS)", com mês de competência obrigatório.
  - Não entra no resultado, porque o resultado usa o imposto estimado.
  - Entra no caixa do mês e na diferença das guias do lucro a dividir.
  - Outras despesas da categoria "Impostos" (IPTU, taxas, alvará) são despesas comuns.
- **Contas:** banco(s), caixa da secretária e maquininha(s), cada uma com saldo inicial na data de início. O saldo é o inicial, mais os recebimentos líquidos, menos os pagamentos, mais ou menos as transferências.
- **Pagamentos** são de cinco tipos: despesa, repasse, repasse anterior à data de início, devolução e divisão de lucros.

### Apuração do mês

Contas feitas para um mês M:

- **Faturamento da linha** = faturamento com competência M, com os valores negativos.
- **Imposto estimado da linha** = alíquota vigente em M × faturamento da linha, arredondado; o imposto do mês é a soma das linhas. A alíquota tem data de vigência, e mudá-la não altera meses passados.
- **Repasse da linha** = repasse devido em M das sessões e avaliações daquela linha.
- **Taxas da linha** = taxas dos recebimentos com data em M cujas cobranças são daquela linha, menos as taxas de recebimentos cancelados em M.
- **Despesas da linha** = despesas com competência M marcadas com a linha.
- **Lucro da linha** = faturamento − imposto estimado − repasse − taxas − despesas da linha.
- **Despesas gerais** = despesas com competência M sem linha.
- Nenhuma conta do resultado inclui as guias do Simples.
- **Resultado** = soma do lucro das linhas − despesas gerais.
- **Por profissional:**
  - **Produção:** o pacote conta para o profissional do pacote, no mês da venda; a sessão avulsa, para quem atendeu; a avaliação, para o profissional dela; o lançamento simples, para o profissional informado.
  - **Lucro do profissional** = produção − imposto estimado sobre ela − repasse devido em M dos pacotes, avaliações e sessões avulsas dele (inclusive o repasse pago a outro parceiro que atendeu um pacote dele) − taxas dos recebimentos dessas cobranças. Despesas não são atribuídas a profissional.
  - O lucro por linha pode ser filtrado por profissional com essa mesma regra (Q7); as despesas da linha só aparecem sem filtro.
- **Caixa do mês** = recebimentos líquidos com data em M − cancelamentos de recebimento com data em M − pagamentos com data em M (despesas, guias, repasses, devoluções). A divisão de lucros e as transferências não entram.
- **Lucro a dividir** de M = os seis itens abaixo, somados:
  - + recebimentos líquidos em M, menos os cancelamentos de recebimento em M;
  - − despesas pagas em M, menos as guias do Simples;
  - − devoluções e repasses anteriores à data de início pagos em M;
  - − repasse devido em M;
  - − imposto estimado de M;
  - − diferença das guias pagas em M.
- **Diferença das guias:** para cada competência C com guia paga em M, é a soma das guias de C pagas até M, menos o imposto estimado de C, menos a diferença de C já considerada em meses anteriores. Guias complementares, multa e juros entram assim pelo valor delas. Guia de competência anterior à data de início não tem estimativa e entra inteira.
- **Na prática**, o lucro a dividir é o caixa do mês com duas trocas:
  - sai o repasse pago no mês (que é de meses anteriores) e entra o repasse devido do próprio mês;
  - sai a guia paga no mês e entra o imposto estimado do próprio mês, mas a diferença entre a guia e a estimativa dela continua descontada.

  A tela mostra essa conta passo a passo.
- **Prejuízo a compensar** (Q37): se o lucro a dividir de um mês for negativo, nada é dividido e o valor passa para os meses seguintes. Ele é descontado do lucro a dividir, antes do fundo de reserva, até zerar.
- **Fundo de reserva** = percentual configurado × lucro a dividir (depois do prejuízo a compensar), quando positivo.
- **Divisão de lucros** = (lucro a dividir − fundo de reserva) × participação de cada sócia (padrão de 25% cada; a soma tem de dar 100%). Os centavos que sobrarem do arredondamento ficam no fundo de reserva.
- **Fechamento do mês** (Q37):
  - Registrar a divisão de lucros de M exige o fechamento de repasse de M, e os meses são fechados em ordem.
  - Ao registrar, faturamento, resultado, caixa do mês, lucro a dividir e divisão de lucros de M ficam congelados.
  - Lançamentos e correções feitos depois, com data ou competência num mês fechado, entram no primeiro mês aberto. Isso vale para recebimentos e seus cancelamentos, despesas, guias, faturamento cancelado e mudanças de sessão. Esses valores aparecem numa linha "ajustes de meses fechados" no resultado e no lucro a dividir, com a lista do que mudou.
- **Pagamentos às sócias:** cada sócia tem o seu pagamento de divisão de lucros registrado, com data e conta.

### Mês de exemplo

Mês de referência aprovado na entrevista; vira teste de aceitação. Imposto de 6% e fundo de reserva de 10%.

**Premissas:**
- Data de início em 01/10/2026, com saldos iniciais zerados; o painel é conferido em 31/10/2026.
- **Taxas:** cartão de crédito com 3%; Pix com 0%.
- **Pagamentos:**
  - Bruno paga o pacote em 01/10, no cartão de crédito.
  - Todos os demais recebimentos são por Pix, um único por cobrança, em outubro.
- **Fábio:** a avaliação é vendida em 05/10. A 1ª parcela vence e é paga em 05/10; a 2ª vence em 05/11.
- **Elisa:** a sessão é em 21/10 e vence nesse dia.
- **Helena:** o aluguel fixo começa em 01/10, com as terças de 6, 13, 20 e 27/10, e a cobrança vence em 05/10.
- **Auditório:** a diária é em 16/10.
- **Despesas:** todas têm vencimento, competência e pagamento em outubro.
- Nenhuma guia nem repasse de mês anterior é pago em outubro.

| # | O que aconteceu | Faturamento | Recebido (líquido) |
|---|---|---:|---:|
| 1 | Ana, paciente da sócia Carla: pacote de 4 sessões, as 4 realizadas | 800 | 800 |
| 2 | Bruno, paciente do parceiro Diego: pacote de 4 sessões no cartão (taxa de R$ 24). Teve 3 sessões realizadas e 1 falta abonada | 800 | 776 |
| 3 | Elisa, paciente do Diego: 1 sessão avulsa realizada e não paga | 200 | 0 |
| 4 | Fábio, avaliação com a parceira Gabriela: R$ 2.500 em 2 parcelas, só a 1ª paga, laudo não entregue | 2.500 | 1.250 |
| 5 | Locatária Helena: aluguel fixo de um turno às terças, 4 no mês | 1.200 | 1.200 |
| 6 | Diária do auditório | 600 | 600 |
| 7 | Supervisão (lançamento simples) | 400 | 400 |
| | **Total** | **6.500** | **5.026** |

Despesas pagas:
- gerais: aluguel do imóvel (1.500), contador (500) e anúncios (300);
- custo direto da Avaliação psicológica e neuropsicológica: folhas de resposta (156).

Valores esperados em outubro:

| | Valor (R$) |
|---|---:|
| Lucro: Psicoterapia e consultas (1.800 − 108 − 180 − 24) | 1.488 |
| Lucro: Avaliação psicológica e neuropsicológica (2.500 − 150 − 156) | 2.194 |
| Lucro: Aluguel de salas e auditório (1.800 − 108) | 1.692 |
| Lucro: Supervisão (400 − 24) | 376 |
| Despesas gerais | −2.300 |
| Resultado | 3.450 |
| Caixa do mês (5.026 − 2.456) | 2.570 |
| Lucro a dividir (5.026 − 2.456 − 180 − 390) | 2.000 |
| Fundo de reserva | 200 |
| Divisão de lucros para cada sócia | 450 |

Outros valores esperados em outubro:
- **Repasse:** Diego com R$ 180 devidos e R$ 60 a liberar (sessão da Elisa); Gabriela com R$ 0 devido e R$ 0 a liberar.
- **Painel:**
  - inadimplência de R$ 200;
  - R$ 1.250 a receber nos próximos 30 dias;
  - crédito de 1 sessão (R$ 200);
  - 6 notas pendentes.
- **Produção:** Carla R$ 800, Diego R$ 1.000, Gabriela R$ 2.500.
- **Lucro por profissional:**
  - Carla: 800 − 48 = 752;
  - Diego: 1.000 − 60 − 180 − 24 = 736;
  - Gabriela: 2.500 − 150 = 2.350.

**Continuação em novembro.** O que acontece:
- a 2ª parcela do Fábio é recebida por Pix em 05/11;
- o repasse de outubro do Diego (180) é pago em 05/11;
- o laudo é entregue em 20/11;
- a guia do Simples de outubro, de R$ 420, é paga em 20/11;
- não há outras vendas nem despesas.

Valores esperados em novembro:
- **Repasse:** Gabriela com R$ 750 devidos (30% × 2.500).
- **Resultado:** −750 (linha de Avaliação: 0 − 0 − 750).
- **Caixa do mês:** 1.250 − 180 − 420 = 650.
- **Lucro a dividir:** 1.250 − 750 − (420 − 390) = 470.
- **Fundo de reserva:** 47; R$ 105,75 para cada sócia.

Os dois meses somam 2.470 de lucro a dividir. É o caixa dos dois meses (3.220) menos o repasse de 750 ainda a pagar.

### Indicadores

O painel tem duas partes.

**Hoje** (sempre na data atual, sem histórico; ver a história 73):
- a receber nos próximos 30 dias e inadimplência;
- a pagar: despesas vencidas, despesas que vencem nos próximos 30 dias e repasses fechados ainda não pagos;
- saldos das contas, conforme o perfil;
- crédito de sessões (quantidade e valor) e notas pendentes;
- repasse provisório e a liberar de cada parceiro;
- lista de espera (quantas pessoas aguardam e há quanto tempo, em média);
- pendências de registro e conflitos a resolver.

**Mês** (escolhendo o mês, com comparação dos últimos 12 meses):
- **Sessões:** realizadas, faltas cobradas, faltas abonadas, canceladas pelo paciente e canceladas pela clínica. Taxa de faltas = (faltas cobradas + faltas abonadas) ÷ (realizadas + faltas). Desmarcação feita no prazo não é falta.
- **Ocupação das salas de aluguel e do auditório** (Q31) = horas de sessões de parceiros e de aluguéis não cancelados nesses espaços ÷ horas de funcionamento do mês nesses espaços.
  - O denominador soma as 4 salas de aluguel e o auditório, sem os dias sem funcionamento.
  - Cada item conta só as horas dentro do funcionamento, para a ocupação nunca passar de 100%.
  - Mostrada no total e por espaço; as horas de bloqueio aparecem à parte.
- **Agenda dos profissionais:**
  - ocupação da agenda de cada sócia: horas de sessões não canceladas na sala dela ÷ horas de funcionamento;
  - para os parceiros: horas de sessões no mês.
- **Pacientes:**
  - Novos: tiveram a 1ª sessão realizada no mês e não estão marcados como anteriores à data de início.
  - Ativos no fim do mês: tiveram sessão realizada nos 30 dias anteriores ao fim do mês ou têm sessão não cancelada nos 30 dias seguintes.
  - Pararam de vir: estavam ativos no fim do mês anterior e não estão no fim deste. Não é calculado no primeiro mês de uso.
- **Contatos:**
  - novos no mês, por origem;
  - convertidos: contatos que chegaram no mês e já viraram paciente, por origem;
  - "não fechou" por motivo;
  - taxa de conversão = convertidos ÷ novos do mês. O número de um mês pode subir depois, à medida que os contatos dele viram pacientes.
- **Lista de espera:** entradas no mês, entradas que passaram a "atendido" no mês e tempo médio de espera delas.
- **Só para as sócias:** faturamento, resultado, lucro por linha (com filtro por profissional), produção e lucro por profissional, caixa do mês, lucro a dividir e divisão de lucros.

### Contatos e lista de espera

- **Contato:** é um registro de procura ligado a uma pessoa, criada já no primeiro contato só com nome e telefone. A mesma pessoa pode ter vários contatos ao longo do tempo.
  - **Campos:** data, origem, o que procura (terapia infantil, adolescente, adulto, casal ou família, avaliação neuropsicológica, avaliação infantil, outro) e situação (novo, 1ª consulta agendada, virou paciente ou não fechou).
  - **Quando a procura é para outra pessoa** (por exemplo, um filho), o contato registra o nome e a data de nascimento dela.
  - **Não fechou** exige um motivo: preço, horário, desistiu ou outro.
  - **Pessoa que já é paciente:** se ela já teve sessão realizada, o sistema avisa isso e não registra contato novo.
- **Avanço:**
  - Agendar a 1ª consulta cria o paciente e marca a sessão.
    - O paciente é a pessoa do contato, ou a outra pessoa informada.
    - Nesse caso, quem procurou vira o responsável financeiro (editável).
  - A situação do contato muda para "1ª consulta agendada".
  - Se a 1ª consulta for cancelada ou desmarcada, o contato volta para "novo".
  - Falta na 1ª consulta não é cobrada: vira falta abonada (Q42).
  - A primeira sessão realizada do paciente muda o contato para "virou paciente", e o paciente leva a origem do contato.
- **Lista de espera:**
  - **Campos de cada entrada:** pessoa (contato ou paciente), o que precisa (mesma lista do contato), dias e horários de preferência, profissional preferido (opcional), data de entrada, situação (aguardando, atendido ou desistiu) e data de saída.
  - Ao marcar uma sessão para quem tem entrada aguardando, o sistema pergunta se a entrada passa a "atendido".
- **Alcance na Fase 1:** só pessoas que procuram psicoterapia ou avaliação. Pessoas que procuram cursos ou aluguel ficam para a Fase 2.

### WhatsApp

Os botões abrem o WhatsApp com o texto pronto, e nada é enviado automaticamente. Os textos são fixos na Fase 1 e usam o nome da clínica, o endereço e a chave Pix das configurações. Há quatro mensagens:
- lembrete ou confirmação da sessão, para o telefone do paciente ou, se ele não tiver, do responsável financeiro;
- agenda do dia ou da semana, para o parceiro;
- resumo do extrato de repasse, para o parceiro (o PDF é baixado e anexado à mão);
- cobrança vencida: uma mensagem por responsável financeiro, juntando todas as cobranças vencidas dele.

### Começo do uso e importação

**Data de início.** É escolhida pelas sócias, por exemplo o dia 1º de um mês. Os saldos iniciais são informados pelas sócias.

**Saldos iniciais:**
- **Contas:** o saldo de cada uma.
- **Cobranças em aberto:** pessoa, linha de receita, valor, vencimento e descrição.
  - Não geram faturamento, mas o recebimento entra no caixa do mês e gera nota pendente normalmente.
  - Quando a cobrança é de sessão de parceiro, informa-se também o parceiro e o percentual, e o recebimento gera repasse.
- **Despesas em aberto.**
- **Pacotes de sessões em andamento:** paciente, responsável financeiro, serviço, profissional, sessões restantes, valor delas e quanto desse valor já foi pago.
  - Não há faturamento.
  - A parte já paga conta como recebida só para o repasse: não gera recebimento nem nota e não entra no caixa do mês, no lucro a dividir nem no saldo das contas.
  - A parte não paga vira parcela em aberto ligada ao pacote.
- **Avaliações em andamento:** paciente, responsável financeiro, profissional, percentual, valor total, valor já recebido (tratado como recebido só para o repasse, como acima) e as parcelas em aberto. As próximas sessões ligam-se a ela, e o laudo é registrado normalmente.
- **Repasse a pagar de antes da data de início**, por parceiro. O pagamento dele é um pagamento próprio, sem fechamento, que entra no caixa do mês e é descontado do lucro a dividir do mês em que for pago.
- **Guia do Simples de competência anterior à data de início:** entra inteira no lucro a dividir do mês em que for paga.

**Pacientes ativos e horários fixos.** Os pacientes são importados ou cadastrados e ficam marcados como anteriores à data de início. Os horários fixos geram sessões só a partir de hoje. O histórico anterior à data de início não é importado.

**Importação de pacientes:** usa uma planilha-modelo em Excel.
- **Colunas:**
  - do paciente: nome, telefone, e-mail, CPF, data de nascimento (opcional), endereço, convênio de desconto e valor combinado da sessão;
  - do responsável financeiro, quando não é o próprio paciente: nome, CPF, telefone, e-mail e endereço.
- **Prévia:** mostra erros e possíveis duplicados (mesmo CPF, ou mesmo nome e telefone, na planilha ou no sistema). Linhas duplicadas, por padrão, não são importadas; quem importa pode marcar "importar mesmo assim". A importação nunca sobrescreve um cadastro existente.
- **Responsável financeiro:** é procurado pelo CPF, ou por nome e telefone, e reaproveitado se já existir. Assim, irmãos com a mesma mãe ficam com um só responsável.
- A planilha nunca é guardada no repositório.

### Relatórios e exportação

**Relatório do contador** (Excel, por mês). Tem as abas:
- **recebimentos:** data, quem pagou, CPF ou CNPJ, descrição, linha, valor bruto, taxa, líquido, forma e nota; incluindo os cancelamentos;
- **repasses fechados por parceiro:** CPF, nota ou RPA, e valor;
- **despesas pagas;**
- **resumo:** faturamento por linha e imposto estimado. Só sai quando uma sócia gera o relatório.

**Exportar tudo** (só sócias):
- gera um Excel com pessoas, configurações e movimentos do NAAP, sem as tabelas de login;
- pede a senha da sócia de novo antes de gerar;
- registra no histórico quem exportou e quando;
- gera o arquivo aos poucos, sem montar tudo na memória, para não esbarrar no limite de tamanho da hospedagem.

### Histórico de alterações

- Toda criação, alteração ou exclusão nestes itens grava quem fez, quando e o que mudou:
  - pessoas, horários fixos, sessões, aluguéis, bloqueios;
  - pacotes, avaliações, lançamentos simples, cobranças, recebimentos e notas, devoluções;
  - despesas, pagamentos, transferências;
  - fechamentos e pagamentos de repasse, divisões de lucros;
  - configurações, usuários, exportações e anonimizações.
- O histórico guarda o identificador da pessoa e do registro, nunca cópias de nome, CPF ou CNPJ, telefone, e-mail, endereço, data de nascimento ou observações. Os valores antes e depois são só os campos administrativos alterados, como situação, datas, valores, sala, profissional e percentual.
- O histórico não pode ser editado nem apagado; o próprio banco de dados impede isso.

### Arquitetura

- **Um único projeto web** em Next.js com TypeScript. As telas são feitas no servidor, e cada ação da usuária chama um **caso de uso** no servidor.
  - **Hospedagem:** Vercel (plano Pro), com a região das funções fixada em São Paulo (gru1), porque o padrão da Vercel é Washington. Nenhuma rota que lê dados usa o runtime Edge.
  - **Banco de dados:** PostgreSQL no Neon, região aws-sa-east-1 (ADR-0003).
- **Três camadas:**
  - **Regras:** funções puras de agenda, crédito, preço, repasse e apuração, sem banco de dados e sem tela.
  - **Casos de uso:** a interface do sistema. Cada ação confere a permissão, aplica as regras e grava tudo numa única transação.
  - **Telas:** só chamam casos de uso e leituras de dados, nunca o banco de dados direto.
- **Conexão com o banco:**
  - Os casos de uso usam o Drizzle com um driver que abre transação: o Pool do driver serverless do Neon ou o node-postgres, pela conexão com pooler. O driver HTTP do Neon não é usado em casos de uso.
  - As migrações usam a conexão direta, sem pooler. Elas são versionadas e escritas em SQL quando o Drizzle não gera o que é preciso, como os intervalos de horário e a restrição de exclusão.
- **Valores e datas:**
  - Dinheiro é guardado em centavos inteiros.
  - Percentuais são aplicados sobre valores acumulados e arredondados ao centavo (meio centavo arredonda para cima). O valor de cada mês ou sessão é a diferença entre dois acumulados.
  - O servidor e o banco de dados rodam em UTC, e o sistema nunca usa a data do relógio do servidor. "Hoje", "mês atual", vencimentos e o dia da sessão vêm de uma única função de datas no fuso America/Fortaleza.
  - Datas sem hora (vencimento, recebimento, despesa, pagamento, competência) são guardadas como data pura.
  - Início e fim de sessões e aluguéis são guardados com fuso. Os intervalos são semiabertos, [início, fim): uma sessão que termina às 9h não choca com outra que começa às 9h.
- **Rotina diária:** cron da Vercel, todo dia às 03h10 UTC (00h10 em Fortaleza). O endereço da rotina exige um segredo próprio.
  - Cada passo recupera o que faltar, em vez de depender de uma data: estende os horários fixos e os aluguéis fixos até 12 semanas à frente e gera as cobranças de aluguel fixo e as despesas recorrentes do mês corrente que ainda não existem.
  - Cada data é gravada à parte: um choque vira conflito sem interromper as demais.
  - A rotina pode rodar duas vezes sem duplicar nada.
- **Login:** better-auth com e-mail e senha, sem cadastro público.
  - A conexão expira depois de 1 hora sem uso (Q45) e é renovada a cada 5 minutos de uso.
  - Usuários nunca são apagados, só desativados, para o histórico continuar mostrando quem fez cada coisa.
  - Desativar uma usuária ou redefinir a senha encerra as conexões abertas dela na hora.
  - Os perfis "sócia" e "secretária" não têm a função de personificar outra usuária.
- **Arquivos:**
  - Os arquivos Excel (relatório do contador, exportar tudo, planilha-modelo e importação) usam a biblioteca exceljs; o pacote xlsx do npm não é usado.
  - O PDF do extrato usa uma biblioteca em JavaScript puro, sem navegador embutido.
- **Publicação:**
  - O ramo principal do GitHub é protegido e só recebe mudanças com a integração contínua verde.
  - A Vercel só promove para produção um build que passou nas mesmas verificações.
  - As migrações rodam uma vez por publicação de produção, num passo que só existe no ambiente de produção, depois de uma cópia manual do banco de dados.

### Segurança, LGPD e operação técnica

- **Ambientes separados:**
  - Produção e demonstração são dois projetos na Vercel e dois no Neon. O banco de dados de demonstração só tem os dados inventados da semente.
  - A conexão com o banco de dados de produção existe só no ambiente de produção da Vercel. As visualizações prévias (previews) usam o banco de dados de demonstração e nunca uma cópia da produção.
  - A rotina diária roda nos dois projetos.
- **Tentativas de senha:**
  - O login passa sempre pela rota HTTP do better-auth.
  - O limite de tentativas é guardado no banco de dados e usa o IP real informado pela Vercel.
  - Além do limite por IP, cada e-mail fica bloqueado por 15 minutos depois de 5 senhas erradas seguidas.
- **Cópias de segurança:**
  - O projeto Neon de produção tem a janela de histórico configurada em 7 dias (o padrão é 1 dia), uma cópia agendada diária e uma cópia manual antes de cada migração.
  - Uma vez por mês, uma sócia usa o "exportar tudo" e guarda o arquivo em lugar seguro.
- **Segredos:**
  - Os segredos (conexões do banco de dados, segredo do login com pelo menos 32 caracteres aleatórios, segredo da rotina) ficam só nas variáveis de ambiente da Vercel, marcadas como sensíveis, com valores diferentes em produção e em demonstração.
  - Nenhum arquivo .env entra no repositório, e a integração contínua procura segredos em todo envio (gitleaks).
  - As contas do GitHub, da Vercel e do Neon usam verificação em duas etapas.
- **Logs sem dados pessoais:**
  - Um tratador único de erros registra só o código do erro, o caso de uso, a restrição violada e um identificador da requisição. A mensagem original de erro do banco de dados não vai para o log.
  - Buscas por nome, telefone ou CPF não vão no endereço da página.
  - A planilha de importação não é registrada em log.
- **Pedidos de titulares (LGPD):**
  - Corrigir dados é a edição normal da pessoa.
  - Anonimizar é exclusivo das sócias: troca o nome por "Pessoa anonimizada nº N" e apaga telefone, e-mail, data de nascimento, endereço e observações.
  - Ficam só os dados exigidos por obrigação legal, ligados a recebimentos e notas.
- **Prazo de guarda:** contatos que não fecharam e entradas encerradas da lista de espera são anonimizados pela rotina diária 24 meses depois da última mudança (ajustável). Os indicadores continuam contando.
- **Papéis e bases legais** (a validar com assessoria jurídica):
  - Controladora: TC2 Serviços em Avaliação Psicológica Ltda (NAAP). Operadoras: Vercel (hospedagem) e Neon (banco de dados). O GitHub guarda só código e dados inventados.
  - Os dados de pessoas, agenda, cobranças e notas são tratados para execução de contrato e por obrigação legal. Por serem dados de saúde, valem as hipóteses do art. 11 da LGPD.
  - A transferência internacional (suporte e subcontratadas dos fornecedores) depende dos contratos de tratamento de dados deles.
  - O aviso de privacidade da clínica deve mencionar o sistema.
- **Incidente de segurança.** Havendo suspeita de acesso indevido, uma sócia:
  - desativa as usuárias envolvidas, o que as desconecta na hora;
  - troca os segredos na Vercel e no Neon;
  - consulta o histórico do dia;
  - decide com assessoria jurídica a comunicação à ANPD e aos titulares, que tem prazo de 3 dias úteis.

### Modelo de dados (conceitual)

**Cadastros:**
- pessoa, com papéis;
- profissional, com percentuais de repasse por serviço;
- usuário, com perfil;
- serviço, sala, auditório, horário de funcionamento, turno e dia sem funcionamento;
- convênio de desconto e valor combinado.

**Agenda:**
- sessão, com data prevista de origem;
- horário fixo;
- aluguel, aluguel fixo e bloqueio;
- conflito a resolver;
- ocupação: tabela técnica única de intervalos de sala e de profissional, com a restrição de exclusão que impede choques.

**Venda e cobrança:**
- pacote de sessões, com o uso do crédito por sessão;
- avaliação em pacote;
- lançamento simples;
- faturamento: registro de receita por linha, mês e profissional, com valores negativos;
- cobrança e parcela;
- recebimento com nota, e cancelamento de recebimento;
- devolução.

**Repasse:**
- fechamento de repasse, com o detalhe congelado por sessão e por avaliação, e saldo a descontar;
- pagamento de repasse;
- repasse anterior à data de início.

**Despesas e contas:**
- despesa e despesa recorrente;
- guia do Simples;
- conta e transferência;
- pagamento.

**Apuração:**
- alíquota com vigência;
- regra de divisão e fundo de reserva com vigência;
- fechamento do mês, com os valores congelados, os ajustes de meses fechados e o prejuízo a compensar;
- pagamentos às sócias.

**Contatos:**
- contato e entrada da lista de espera.

**Histórico:**
- registro de alterações, só de inclusão.

### Valores iniciais na instalação

- **Salas:** 4 salas de sócia (uma por sócia), 4 salas de aluguel e 1 auditório.
- **Funcionamento e turnos:**
  - segunda a sexta das 7h às 19h, com turnos de 7h às 13h e de 13h às 19h;
  - sábado das 6h às 12h, com um turno.
- **Serviços:**
  - "Sessão de psicoterapia" (tipo sessão, R$ 200, 50 min);
  - "Avaliação — pacote completo" (tipo avaliação em pacote, R$ 2.500, sessões de 50 min);
  - "Avaliação — sessão avulsa" (tipo sessão avulsa de avaliação, R$ 350, 50 min).
  - As sócias acrescentam os demais.
- **Agenda e cadastro:**
  - repasse padrão de 30%;
  - regra das 24 horas;
  - horizonte de 12 semanas para horários e aluguéis fixos;
  - 30 dias para considerar o paciente ativo;
  - 1 hora sem uso até desconectar;
  - 24 meses de guarda de contatos não fechados.
- **Aluguel:**
  - R$ 50 a hora, R$ 300 o turno e R$ 600 a diária do auditório;
  - turno do auditório no sábado com preço informado pelas sócias;
  - vencimento do aluguel fixo no dia 5.
- **Divisão de lucros:** 25% para cada sócia. Fundo de reserva de 0%, ajustável.
- **Categorias de despesa:**
  - aluguel e condomínio; energia, água, internet e telefone;
  - pessoal da secretária; pró-labore; impostos;
  - marketing e anúncios; material e testes; limpeza e copa;
  - manutenção; contador e serviços; sistemas e assinaturas;
  - tarifas bancárias; custos de cursos; outras.
- **Origens:** Instagram, Google, site, indicação de paciente, indicação de profissional, CAACE e outra.
- **Convênio de desconto:** CAACE.
- **Informados pelas sócias na instalação:**
  - alíquota estimada, que vem do contador;
  - percentual do desconto CAACE;
  - taxas da maquininha por forma de recebimento;
  - contas, chave Pix e endereço;
  - dias sem funcionamento do ano.

## Decisões de teste

- **Um bom teste** confere o comportamento visível pela interface do sistema (os casos de uso e as leituras de dados), nunca detalhes internos. Os nomes dos testes usam os termos do glossário.
- **Ponto de teste das regras de negócio: a camada de casos de uso.** Os testes rodam contra um PostgreSQL real em memória (PGlite), com a extensão btree_gist e as mesmas migrações de produção, aplicadas pelo mesmo comando, nunca por sincronização automática do esquema.
- **Funções puras** de datas, turnos e arredondamento também podem ter testes unitários próprios, quando isso deixar o erro mais claro.
- **Testes de aceitação** obrigatórios:
  - **O mês de exemplo e a continuação em novembro**, montados só com casos de uso, conferindo cada valor esperado. Isso inclui o lucro por profissional e o lucro a dividir dos dois meses.
  - **Repasse:**
    - base bruta: pacote pago em parte no cartão (R$ 300 → R$ 90 devidos e R$ 90 a liberar), e pacote pago no cartão com as 4 sessões realizadas (R$ 240);
    - pacote com sessões de sócia e de parceiros com percentuais diferentes;
    - avaliação com laudo e parcela recebida depois; avaliação encerrada sem laudo;
    - correção depois do fechamento virando ajuste; fechamento negativo virando saldo a descontar;
    - arredondamento de R$ 1.000 em 3 sessões (100 / 100 / 100 de repasse);
    - saldo inicial de pacote: 2 sessões restantes de R$ 400 já pagas geram R$ 120 de repasse e nenhum valor no caixa do mês.
  - **Apuração:**
    - cancelamento de recebimento de mês fechado entrando no mês do cancelamento;
    - guia complementar da mesma competência (420 no mês 2 → +30; 15 no mês 3 → +15);
    - despesa de IPTU na categoria "Impostos" tratada como despesa comum;
    - prejuízo a compensar seguido de mês positivo;
    - lançamento com data num mês fechado virando "ajuste de meses fechados";
    - recebimento lançado às 21h30 do último dia do mês, no horário de Fortaleza, caindo nesse mês.
  - **Agenda:**
    - choque de sala e de profissional pela conferência do caso de uso, e a recusa pelo próprio banco de dados quando essa conferência é pulada. É isso que prova a restrição de exclusão, já que o PGlite executa uma transação por vez;
    - horário fixo com data em choque e em dia sem funcionamento;
    - alterar e encerrar horário fixo; rotina depois de remarcar uma sessão não recria a data original;
    - regra das 24 horas (exatamente 24 horas e aviso depois do início), abono, desfazer, remarcar, cancelar pela clínica e sessão desmarcada liberando o horário.
  - **Crédito:**
    - uso do crédito só do mesmo serviço;
    - crédito esgotado virando sessão avulsa;
    - devolução com parcela em aberto.
  - **Aluguel:**
    - aluguel fixo criado no meio do mês;
    - rotina que não rodou no dia 1º;
    - data cancelada com e sem 24 horas, com cobrança aberta e paga;
    - ocupação sem passar de 100%.
  - **Permissões:** a secretária é barrada em cada caso de uso e leitura exclusivos das sócias, inclusive a aba de resumo do relatório e o saldo do banco.
  - **Importação e começo do uso:**
    - planilha válida, inválida e com duplicados;
    - responsável financeiro reaproveitado;
    - pacientes anteriores não contando como novos.
  - **Rotina diária:** rodando duas vezes sem duplicar sessões, aluguéis, cobranças nem despesas.
- **Testes de ponta a ponta** (Playwright, no navegador) cobrem poucos fluxos principais pela tela:
  - login e desconexão por falta de uso;
  - marcar e registrar sessão;
  - vender pacote;
  - registrar recebimento;
  - fechar repasse;
  - ver o painel.
- **Integração contínua no GitHub:** verificação de estilo, checagem de tipos, testes, busca de segredos e build a cada envio. A publicação em produção só acontece com tudo verde.
- **Exemplos anteriores:** nenhum; o repositório começa do zero.

## Fora do escopo

- Prontuário e qualquer conteúdo clínico (ADR-0001).
- Planos de saúde e faturamento de convênio médico.
- Acesso de parceiros, pacientes ou contador; portal do paciente; agendamento online.
- Envio automático de WhatsApp (Fase 3) e textos de mensagem editáveis.
- Emissão de nota fiscal pelo sistema (Fase 3, com o Emissor Nacional).
- Importação de extrato bancário e conciliação (Fase 3). Acompanhamento do prazo de depósito da maquininha (fase a definir).
- Gateway de pagamento e cobrança automática por Pix ou boleto.
- Módulos próprios de aluguel de testes com estoque, cursos e turmas, compras e estoque, e custo por paciente de cada canal de marketing (Fase 2). Na Fase 1, essas receitas entram como lançamento simples.
- Contatos de pessoas que procuram cursos ou aluguel (Fase 2).
- Divisão de despesas gerais entre linhas de receita ou entre profissionais.
- Cálculo de retenções de RPA (INSS, IR, ISS), que fica com o contador.
- Cobrança de sessões no fim do mês (pós-paga): sessões só são cobradas por pacote ou como sessão avulsa.
- Incluir sessões avulsas já cobradas num pacote vendido depois.
- Desconto do aluguel de um parceiro no repasse dele (Q44).
- Mais de uma unidade, aplicativo de celular nativo, verificação em duas etapas no sistema e recuperação de senha por e-mail.
- Demonstrativos contábeis oficiais e folha de pagamento.

## Notas adicionais

- **Custo mensal estimado:** R$ 135 a 160. São US$ 20 da Vercel Pro mais cerca de US$ 5 a 10 do Neon, com o dólar a R$ 5,12 e IOF; o projeto de demonstração no Neon fica no plano gratuito. O valor exato é confirmado ao contratar as assinaturas.
- **Ações que só as sócias podem fazer:**
  - deixar o repositório do GitHub privado antes do primeiro código do sistema (Q46);
  - contratar as assinaturas da Vercel e do Neon no nome da clínica, com cartão da empresa;
  - ativar a verificação em duas etapas no GitHub, na Vercel e no Neon;
  - ligar a Vercel ao repositório;
  - opcionalmente, pedir o subdomínio sistema.naappsicologia.com.br a quem cuida do site.

  O passo a passo de cada ação é entregue junto com a primeira entrega do site de demonstração.
- **Atualizações de segurança** (Q46): o GitHub avisa sobre falhas nas bibliotecas e abre pedidos de atualização toda semana. Uma sócia recebe esses avisos por e-mail e abre uma sessão de desenvolvimento para aplicá-los. Correções críticas são publicadas no mesmo dia; as demais, em até 48 horas.
- **Ordem de construção:** a Fase 1 será construída em entregas que as sócias testam no site de demonstração, com dados inventados, antes da versão oficial. A ordem fica no plano de implementação.
- **Próximas fases:**
  - **Fase 2:** aluguel de testes com estoque de folhas de resposta, cursos e turmas, compras e estoque, e custo por paciente de cada canal.
  - **Fase 3:** WhatsApp automático, emissão de nota pelo Emissor Nacional e importação de extrato bancário.
  - Cada fase terá a sua rodada de perguntas antes de começar.
