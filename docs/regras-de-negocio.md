# Clareza — Regras de Negócio

**Versão:** 1.3  
**Status:** Consolidado com decisões pendentes explicitadas.

## A. Conta, autenticação e segurança

- **RN01.** Cada usuário deve possuir um identificador único.
- **RN02.** O e-mail de usuário deve ser único.
- **RN03.** A senha nunca pode ser armazenada em texto puro ou em formato reversível; deve ser armazenado um hash seguro.
- **RN04.** O usuário pode alterar sua própria senha após autenticação.
- **RN05.** A alteração de e-mail só é concluída após confirmação do novo endereço.
- **RN06.** O backend deve identificar o usuário a partir do contexto autenticado, e não confiar em um identificador de usuário informado pelo cliente para determinar propriedade.
- **RN07.** Toda leitura ou alteração protegida deve validar autenticação, propriedade e autorização no backend.
- **RN08.** Ser administrador de família não concede, por si só, acesso aos registros financeiros individuais dos membros.
- **RN09.** Segredos, credenciais e chaves não podem ser versionados no repositório.
- **RN10.** Em produção, a comunicação deve ser protegida por HTTPS.
- **RN11.** A base de dados não deve ficar exposta diretamente à internet.
- **RN12.** Dados persistidos e backups devem receber proteção por controles de acesso e criptografia apropriados.

## B. Categorias

- **RN13.** O sistema deve oferecer categorias predefinidas e permitir categorias próprias.
- **RN14.** Categoria predefinida é identificada por `id_usuario` nulo; categoria própria pertence a um usuário.
- **RN15.** O usuário não pode editar nem excluir categorias predefinidas.
- **RN16.** O usuário pode desativar uma categoria própria utilizada em registros históricos.
- **RN17.** Categoria inativa não pode ser selecionada em novos registros.
- **RN18.** A desativação de categoria não pode apagar nem alterar os registros históricos que a referenciam.
- **RN19.** O contexto do registro define sua natureza financeira; não é necessário manter um campo genérico de tipo em `CATEGORIA`.

## C. Entradas e valores monetários

- **RN20.** Cada entrada pertence a exatamente um usuário.
- **RN21.** Cada entrada deve referenciar uma categoria válida para o usuário ou uma categoria predefinida.
- **RN22.** O valor de uma entrada deve ser maior que zero.
- **RN23.** A data da entrada é uma data de calendário, sem horário, no MVP.
- **RN24.** A descrição da entrada é opcional.
- **RN25.** Valores monetários devem ser armazenados com tipo decimal adequado; não usar FLOAT/DOUBLE para valores financeiros.
- **RN26.** O backend deve validar valores, datas, relações e permissões, ainda que o frontend também valide.

## D. Investimentos

- **RN27.** Cada investimento pertence a um usuário e a uma categoria.
- **RN28.** Cada investimento pode possuir várias movimentações.
- **RN29.** Os tipos de movimentação no MVP são `APLICACAO` e `RESGATE`.
- **RN30.** O valor de cada movimentação deve ser maior que zero; o tipo determina o sentido financeiro.
- **RN31.** Um resgate não pode exceder o saldo líquido registrado do investimento.
- **RN32.** O saldo líquido registrado corresponde à soma das aplicações menos a soma dos resgates.
- **RN33.** Investimento não é contado como gasto comum nem como dinheiro disponível no caixa.
- **RN34.** Sem dados de mercado, rentabilidade ou valorização, o sistema não deve apresentar o saldo líquido de movimentações como se fosse o valor de mercado atual.
- **RN35.** O conjunto de estados de `INVESTIMENTO.status` ainda precisa ser decidido; não presumir uma enumeração definitiva.

## E. Família e código de ingresso

- **RN36.** Cada família possui identificador interno e código de ingresso único.
- **RN37.** O código de ingresso deve ser difícil de adivinhar e não pode substituir o identificador interno.
- **RN38.** O código permite solicitar ingresso; não concede acesso automaticamente.
- **RN39.** Somente um administrador ativo pode aprovar ou recusar solicitação de ingresso.
- **RN40.** Um administrador pode regenerar o código de ingresso.
- **RN41.** Ao regenerar o código, o código antigo deve deixar de funcionar imediatamente.
- **RN42.** Regenerar o código não remove membros existentes nem cancela automaticamente solicitações já registradas.
- **RN43.** O nome da família é definido na criação e pode ser alterado por administrador ativo.
- **RN44.** O nome da família não precisa ser único.
- **RN45.** A família continua existindo mesmo se o usuário que a criou sair.
- **RN46.** Uma família pode ficar temporariamente sem chefe.
- **RN47.** Uma família deve manter pelo menos um administrador ativo.

## F. Participação e histórico de família

- **RN48.** A participação do usuário em uma família é representada por `MEMBRO`, separadamente da conta `USUARIO`.
- **RN49.** `data_saida` nula indica participação ativa; preenchida indica participação encerrada.
- **RN50.** Um usuário pode ter várias participações históricas, mas no máximo uma participação ativa por vez.
- **RN51.** Ao sair ou ser removido, a participação deve ser encerrada, não apagada.
- **RN52.** Reingresso em uma família cria uma nova participação `MEMBRO`.
- **RN53.** Sair ou ser removido não exclui a conta do usuário nem seus registros financeiros pessoais.
- **RN54.** Um usuário removido pode solicitar ingresso novamente, sujeito ao fluxo normal de aprovação.
- **RN55.** Aprovar uma solicitação e criar a participação ativa devem ocorrer na mesma transação.
- **RN56.** Não pode haver duas solicitações pendentes simultâneas para o mesmo par usuário/família.
- **RN57.** O usuário não pode ingressar em uma nova família enquanto possuir outra participação ativa.
- **RN58.** O banco e o backend devem impedir participações ativas simultâneas para o mesmo usuário, mesmo diante de requisições concorrentes.

## G. Solicitações de ingresso

- **RN59.** Uma solicitação pertence a exatamente um usuário e a exatamente uma família.
- **RN60.** Os status previstos são `PENDENTE`, `APROVADA`, `RECUSADA` e `CANCELADA`.
- **RN61.** Solicitações concluídas devem ser preservadas para histórico.
- **RN62.** Solicitação aprovada não pode ser aprovada novamente.
- **RN63.** Não se pode aprovar solicitação se o usuário já possuir participação familiar ativa.
- **RN64.** A política de quem pode cancelar uma solicitação pendente e suas transições exatas permanece pendente de decisão.
- **RN65.** Enquanto a regra de cancelamento não for decidida, não implementar uma permissão de cancelamento baseada em suposição.

## H. Funções e permissões de família

- **RN66.** As funções de domínio previstas são `CRIADOR`, `ADMINISTRADOR`, `CHEFE` e `MEMBRO`.
- **RN67.** As funções são associadas à participação em família, e não diretamente à conta do usuário.
- **RN68.** A função `ADMINISTRADOR` é atribuída automaticamente ao criador da família.
- **RN69.** Ainda precisa ser confirmado se o criador recebe também a função `CRIADOR`.
- **RN70.** Uma família pode ter vários administradores ativos.
- **RN71.** Não é permitido remover, rebaixar ou encerrar a participação do último administrador ativo sem antes promover outro administrador.
- **RN72.** Somente administrador ativo pode atribuir ou remover a função `ADMINISTRADOR`.
- **RN73.** Somente administrador ativo pode atribuir ou remover a função `CHEFE`.
- **RN74.** Pode haver no máximo um chefe ativo por família.
- **RN75.** `CHEFE` e `ADMINISTRADOR` são funções independentes; nenhuma implica automaticamente a outra.
- **RN76.** O chefe pode sair sem indicar substituto; a família passa a ficar sem chefe.
- **RN77.** Se um administrador desejar sair, deve permanecer outro administrador ativo ou alguém deve ser promovido antes da saída.
- **RN78.** A política de conflito e de remoção/rebaixamento de um administrador por outro ainda precisa ser definida em detalhe.
- **RN79.** A remoção de um membro por administrador encerra a participação e preserva o histórico.
- **RN80.** Remover o chefe não obriga a designar outro chefe imediatamente.
- **RN81.** A remoção de membro não apaga os dados financeiros pessoais do membro.

## I. Acesso aos dados familiares

- **RN82.** Cada usuário pode consultar seus próprios dados financeiros, respeitadas as regras gerais de autenticação.
- **RN83.** O chefe pode consultar os dados financeiros individuais dos membros ativos da família, conforme autorização aplicada no backend.
- **RN84.** Um membro comum não pode consultar os registros financeiros individuais dos demais membros apenas por pertencer à família.
- **RN85.** Membros podem consultar a saúde financeira agregada da família mesmo sem existir chefe.
- **RN86.** Ser administrador não concede acesso automático às finanças individuais.
- **RN87.** Após saída ou remoção, o usuário perde o acesso baseado naquela participação familiar ativa; o histórico e os dados pessoais permanecem.
- **RN88.** O backend deve verificar família, participação ativa e função em cada operação autorizada, sem confiar apenas na interface.

## J. Compras e pagamentos

- **RN89.** Cada compra pertence a um usuário e a uma categoria.
- **RN90.** O valor total da compra deve ser maior que zero.
- **RN91.** Uma compra possui uma ou mais linhas de pagamento em `PAGAMENTO_COMPRA`.
- **RN92.** Cada linha de pagamento pertence a exatamente uma compra.
- **RN93.** A soma dos valores das linhas de pagamento deve ser igual ao valor total da compra antes de confirmar o registro.
- **RN94.** Uma compra pode ser dividida entre meios de pagamento diferentes e entre vários cartões.
- **RN95.** A forma de pagamento é registrada em cada linha de pagamento, não como atributo único da compra.
- **RN96.** As formas iniciais são `PIX`, `DINHEIRO`, `DEBITO` e `CREDITO`.
- **RN97.** Uma linha `CREDITO` deve referenciar um cartão válido e exige autorização ativa do usuário que registra o compromisso.
- **RN98.** Uma linha não creditícia não deve referenciar cartão.
- **RN99.** A compra só deve ser confirmada quando todas as partes de pagamento estiverem definidas e a soma estiver validada.
- **RN100.** As gravações da compra e de seus pagamentos/parcelas devem ser transacionais.
- **RN101.** Pagamentos por PIX, dinheiro ou débito não geram parcelas futuras.
- **RN102.** Pagamento no crédito gera as parcelas definidas para aquela parte do pagamento, inclusive uma parcela para crédito sem parcelamento.
- **RN103.** A soma das parcelas de uma parte paga no crédito deve corresponder ao valor daquela parte do pagamento, salvo regra futura explicitamente definida para arredondamentos.
- **RN104.** A mesma compra pode gerar parcelas em cartões diferentes, com quantidades de parcelas diferentes.
- **RN105.** A compra é contabilizada uma única vez nos gráficos de gastos; parcelas representam compromissos de fatura, não novas compras.
- **RN106.** A regra de edição, exclusão ou retificação de compra que já tenha parcelas ou faturas relacionadas ainda precisa ser definida.

## K. Cartões e usuários autorizados

- **RN107.** Cada cartão tem um único titular cadastrado no Clareza.
- **RN108.** O titular do Clareza não é uma verificação de titularidade legal junto ao emissor.
- **RN109.** Um cartão pode ter vários usuários autorizados.
- **RN110.** A autorização de usuário-cartão é representada por `USUARIO_CARTAO`.
- **RN111.** Um usuário autorizado pode utilizar o cartão mesmo que pertença a outra família ou não pertença à família do titular.
- **RN112.** O limite total do cartão é compartilhado; não é duplicado por usuário autorizado.
- **RN113.** A autorização deve estar ativa no momento de registrar novo compromisso no crédito.
- **RN114.** Revogar autorização bloqueia novas utilizações, mas não apaga compras, parcelas ou compromissos existentes.
- **RN115.** Os valores válidos de `USUARIO_CARTAO.status` ainda precisam ser definidos.
- **RN116.** A visibilidade de compras, parcelas e faturas entre titular, usuário que lançou a compra e demais autorizados, especialmente entre famílias diferentes, ainda precisa ser decidida.

## L. Parcelas e faturas

- **RN117.** Cada parcela pertence a exatamente uma fatura de cartão.
- **RN118.** Cada parcela deve ter exatamente uma origem: `PAGAMENTO_COMPRA` ou `OCORRENCIA_DESPESA`; nunca ambas e nunca nenhuma.
- **RN119.** O valor da parcela deve ser maior que zero.
- **RN120.** Cada fatura pertence a exatamente um cartão.
- **RN121.** Deve existir no máximo uma fatura por cartão e mês de referência.
- **RN122.** O total da fatura é calculado pela soma das parcelas relacionadas.
- **RN123.** O total pago da fatura é calculado pela soma de `PAGAMENTO_FATURA`.
- **RN124.** O saldo restante é calculado como total da fatura menos total pago, sujeito à política de sobrepagamento ainda pendente.
- **RN125.** Total, total pago e saldo restante não devem ser editados diretamente como campos independentes.
- **RN126.** Uma fatura pode ter vários pagamentos, incluindo pagamentos parciais.
- **RN127.** O MVP não calcula juros, multas, tarifas ou encargos cobrados pelo emissor.
- **RN128.** Uma fatura vencida com saldo não quitado pode ser indicada como atrasada, mas a regra exata de status derivado precisa ser formalizada junto aos estados da fatura.
- **RN129.** O ciclo de fechamento e a atribuição de parcela à fatura, inclusive em datas de fechamento, meses curtos e vencimentos, precisam de regra definitiva.
- **RN130.** A política para sobrepagamento, estorno, chargeback e correção de pagamentos de fatura ainda precisa ser decidida.

## M. Despesas recorrentes e ocorrências

- **RN131.** Cada despesa recorrente pertence a um usuário e a uma categoria.
- **RN132.** As periodicidades iniciais são `SEMANAL`, `MENSAL` e `ANUAL`.
- **RN133.** O status da configuração de recorrência é `ATIVA` ou `INATIVA`.
- **RN134.** Uma recorrência ativa deve manter ocorrências geradas para os próximos seis meses, com janela móvel.
- **RN135.** Não pode existir mais de uma ocorrência da mesma recorrência para a mesma data de vencimento.
- **RN136.** Cada ocorrência pertence a exatamente uma configuração de despesa recorrente.
- **RN137.** A ocorrência armazena seu próprio valor previsto para preservar o histórico.
- **RN138.** Alterar o valor da recorrência não altera ocorrências passadas nem ocorrências já pagas.
- **RN139.** Ocorrências futuras ainda pendentes podem ser atualizadas a partir da data efetiva escolhida.
- **RN140.** Desativar uma recorrência interrompe a geração de novas ocorrências, sem apagar o histórico.
- **RN141.** Os status de ocorrência são `PENDENTE`, `PAGA` e `CANCELADA`.
- **RN142.** Ocorrência pendente pode não ter dados de pagamento.
- **RN143.** Ao marcar ocorrência como paga, devem ser registrados data de pagamento/lançamento, valor efetivo e forma de pagamento.
- **RN144.** No MVP, uma ocorrência usa uma única forma de pagamento.
- **RN145.** Se paga por crédito, a ocorrência exige cartão autorizado e gera parcela(s) vinculada(s) à ocorrência e à(s) fatura(s).
- **RN146.** Se paga por PIX, dinheiro ou débito, não gera parcelas e o cartão deve ser nulo.
- **RN147.** Ocorrência cancelada não deve ser contada como despesa realizada.
- **RN148.** Para pagamento no crédito, o significado exato de `data_pagamento` deve ser esclarecido: provavelmente representa a data do lançamento/compra no cartão, e não o pagamento da fatura.
- **RN149.** A política de edição ou cancelamento de ocorrência que já tenha parcelas geradas precisa preservar a integridade da fatura e permanece pendente.

## N. Saúde financeira e relatórios

- **RN150.** A saúde financeira individual é um indicador dinâmico derivado de dados financeiros do usuário.
- **RN151.** A saúde financeira familiar é um indicador dinâmico agregado, não uma informação fixa digitada por um membro.
- **RN152.** O cálculo pode considerar a proporção de saídas em relação às entradas e a evolução da capacidade de poupar, entre outros indicadores que venham a ser definidos.
- **RN153.** Os rótulos ilustrativos discutidos incluem “se recuperando”, “sentindo sintomas ruins”, “dor de cabeça”, “enxaqueca pesada” e “saúde de ferro”; os limites, pesos e textos finais ainda não foram definidos.
- **RN154.** A saúde familiar pode ser exibida sem chefe de família.
- **RN155.** O sistema deve exibir histórico, mês atual e expectativa do mês seguinte.
- **RN156.** A expectativa inicial deve ser calculada com compromissos conhecidos, incluindo parcelas futuras e ocorrências recorrentes previstas.
- **RN157.** A mesma obrigação não pode ser contada duas vezes na projeção.
- **RN158.** A projeção do mês seguinte não é garantia nem previsão por IA.
- **RN159.** Investimentos devem permanecer separados das despesas comuns e do caixa disponível nos indicadores financeiros.

## O. Integridade, transações e cálculos

- **RN160.** As operações compostas devem ser atômicas: ou todas as gravações relacionadas são confirmadas, ou nenhuma é confirmada.
- **RN161.** Aprovar solicitação e criar participação devem ocorrer na mesma transação.
- **RN162.** Registrar compra, pagamentos e parcelas deve ocorrer na mesma transação.
- **RN163.** Marcar ocorrência como paga e gerar parcelas/fatura, quando aplicável, deve ocorrer na mesma transação.
- **RN164.** Totais derivados devem ser calculados no backend/banco a partir das fontes de verdade, não aceitos como verdade do frontend.
- **RN165.** Restrições de unicidade, chaves estrangeiras, checks e índices devem ser usados quando suportados pelo banco e complementados por validação no backend.
- **RN166.** A concorrência deve ser considerada em operações como promoção do último administrador, criação de participação ativa, autorização de cartão e resgate de investimento.
- **RN167.** O sistema deve preservar dados históricos necessários para explicar compras, parcelas, faturas, pagamentos, investimentos e participações anteriores.

## Pendências que não devem ser resolvidas por suposição

- Cancelamento de solicitação pendente e quem pode executá-lo.
- Atribuição da função `CRIADOR` ao criador, além de `ADMINISTRADOR`.
- Política detalhada de conflito entre administradores.
- Valores de `INVESTIMENTO.status` e `USUARIO_CARTAO.status`.
- Permissões de visualização de cartões, compras e faturas entre titular e usuários autorizados.
- Calendário exato de fechamento, vencimento e atribuição de parcelas às faturas.
- Sobrepagamento, estornos, chargebacks e correções de fatura.
- Unicidade de nomes de categorias.
- Fórmula e limiares da saúde financeira.
- Calendário e fuso horário de recorrências.
- Significado exato da data de pagamento de ocorrência no crédito.
- Retificação/exclusão de compras ou ocorrências que já tenham parcelas relacionadas.
