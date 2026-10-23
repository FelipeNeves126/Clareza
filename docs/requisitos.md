# Clareza — Documento de Requisitos

**Versão:** 1.3  
**Status:** Base consolidada do MVP; decisões em aberto identificadas ao final.

## 1. Visão geral

Clareza é uma aplicação web de controle financeiro pessoal e familiar. No MVP, os lançamentos são informados manualmente. A aplicação organiza entradas, compras, investimentos, despesas recorrentes, compras no crédito, parcelas e faturas, além de oferecer indicadores de saúde financeira individual e familiar.

O projeto também tem finalidade de aprendizagem e portfólio profissional: deve demonstrar desenvolvimento full-stack, API REST, persistência relacional, autenticação, autorização, integridade transacional e documentação de domínio.

## 2. Objetivos

- Registrar e consultar entradas financeiras.
- Registrar compras e saídas com pagamento imediato, crédito à vista ou parcelado e divisão entre diferentes meios/cartões.
- Controlar despesas recorrentes por meio de ocorrências previstas.
- Acompanhar parcelas e faturas de cartões, incluindo pagamentos parciais de fatura.
- Registrar investimentos separadamente do dinheiro disponível e das despesas comuns.
- Permitir que usuários criem ou ingressem em uma família por código, com aprovação de administradores.
- Preservar o histórico de participação de usuários em famílias.
- Controlar funções de membro, administração e chefia sem confundir permissões administrativas com acesso aos dados financeiros.
- Exibir saúde financeira individual e familiar de forma dinâmica.
- Exibir histórico e projeção do mês seguinte com base em compromissos conhecidos.
- Proteger os dados por autenticação, autorização e práticas seguras de armazenamento.

## 3. Escopo do MVP

### 3.1 Conta e autenticação

- Cadastro de usuário com nome, data de nascimento, e-mail e senha.
- Login e logout.
- Armazenamento de senha somente como hash seguro; nunca em texto puro.
- Alteração da própria senha após autenticação.
- Alteração de e-mail somente após confirmação do novo endereço.
- Identificação do usuário autenticado no backend; o backend não deve confiar em um `id_usuario` enviado pelo frontend para determinar a propriedade dos dados.

### 3.2 Categorias

- Categorias predefinidas pela aplicação.
- Categorias próprias criadas pelo usuário.
- Desativação de categoria própria sem apagar os registros históricos que a utilizam.
- Categorias inativas não podem ser escolhidas em novos registros, mas continuam visíveis em registros históricos.
- Usuários não podem editar nem excluir categorias predefinidas.

### 3.3 Entradas

- Cadastro manual de entrada.
- Cada entrada pertence a um usuário e a uma categoria.
- Data da entrada é uma data de calendário, sem horário, no MVP.
- Valor deve ser maior que zero.
- Descrição é opcional.

### 3.4 Compras e pagamentos

- Cadastro de compras com descrição, data, categoria e valor total.
- Uma compra pode ser dividida entre vários pagamentos.
- Formas de pagamento iniciais: PIX, dinheiro, débito e crédito.
- Uma compra pode ser dividida entre mais de um cartão, inclusive com quantidades de parcelas diferentes por cartão.
- O registro completo da compra só é confirmado quando todas as partes do pagamento estiverem definidas e a soma delas for igual ao total da compra.
- PIX, dinheiro e débito não geram parcelas futuras.
- Pagamento no crédito gera parcelas, inclusive uma única parcela quando a compra é feita no crédito sem parcelamento.
- Uma compra deve ser contabilizada uma única vez nos gráficos de gastos; suas parcelas não podem ser somadas novamente como se fossem novas compras.

### 3.5 Cartões, parcelas e faturas

- Cadastro de cartão com titular dentro do Clareza, nome identificador, limite total, dia de fechamento e dia de vencimento.
- O titular é o titular cadastrado no Clareza; o MVP não verifica titularidade jurídica junto à instituição financeira.
- Um cartão pode ter vários usuários autorizados, inclusive usuários que não pertencem à mesma família do titular.
- A autorização de uso é controlada por vínculo entre usuário e cartão.
- Revogar autorização impede novas utilizações, mas não apaga compras, parcelas ou compromissos já registrados.
- Cada parcela pertence a uma única fatura e a uma única origem: pagamento de compra ou ocorrência de despesa recorrente.
- Cada cartão tem faturas mensais; uma fatura pertence a um único cartão.
- Faturas aceitam um ou mais pagamentos, inclusive pagamentos parciais.
- Total da fatura, total pago e saldo restante são calculados a partir das parcelas e dos pagamentos registrados, não editados diretamente.
- O MVP não calcula juros, multas, tarifas ou encargos financeiros de instituições emissoras.

### 3.6 Despesas recorrentes

- Cadastro de despesa recorrente com categoria, descrição, valor, periodicidade, início, fim opcional e status.
- Periodicidades iniciais: semanal, mensal e anual.
- Uma despesa recorrente ativa gera ocorrências para os próximos seis meses, mantendo uma janela móvel de seis meses.
- Não podem ser geradas ocorrências duplicadas para a mesma despesa recorrente e data de vencimento.
- Cada ocorrência preserva o valor previsto para seu período.
- Ao alterar o valor da recorrência, ocorrências passadas e ocorrências já pagas não são alteradas. Ocorrências futuras ainda pendentes podem ser atualizadas a partir da data efetiva definida.
- Desativar uma recorrência impede novas gerações, sem apagar o histórico.
- Uma ocorrência pode estar pendente, paga ou cancelada.
- Ao marcar uma ocorrência como paga, registram-se data, valor efetivamente pago e forma de pagamento.
- No MVP, cada ocorrência usa uma única forma de pagamento.
- Se a ocorrência for paga no crédito, exige cartão autorizado e gera parcela(s) vinculada(s) à ocorrência e à(s) fatura(s) correspondente(s).
- Se for paga por PIX, dinheiro ou débito, não gera parcelas e não informa cartão.

### 3.7 Investimentos

- Cadastro de investimento vinculado a usuário e categoria.
- Registro de movimentações do tipo aplicação ou resgate.
- Valores das movimentações devem ser positivos; o tipo define o sentido da movimentação.
- Resgates não podem exceder o saldo líquido registrado (aplicações menos resgates).
- Investimentos não fazem parte do caixa disponível e não são tratados como despesas comuns.
- Sem integração com mercado, rentabilidade ou valorização, o saldo exibido representa o total líquido de aplicações e resgates registrados, não necessariamente o valor de mercado atual.

### 3.8 Famílias, participação e funções

- Um usuário pode ter histórico de participação em várias famílias, mas somente uma participação ativa por vez.
- Uma família tem código de ingresso que permite solicitar entrada; o código não concede acesso automaticamente.
- A entrada depende da aprovação de um administrador ativo.
- Solicitações têm status e histórico próprios.
- Aprovar uma solicitação e criar a participação ativa devem ocorrer na mesma transação.
- Ao sair ou ser removido, a participação é encerrada, preservando-se seu histórico.
- Reingresso cria uma nova participação; não reativa a participação antiga.
- Sair de uma família não exclui a conta nem os registros financeiros pessoais.
- Uma família pode ficar sem chefe, mas deve sempre manter pelo menos um administrador ativo.
- Uma família pode ter vários administradores e no máximo um chefe ativo.
- A função de chefe não implica automaticamente a função de administrador, e vice-versa.
- O criador da família recebe automaticamente a função de administrador; a atribuição adicional da função `CRIADOR` precisa ser confirmada.
- Administradores gerenciam a família e as solicitações, mas não recebem automaticamente acesso às finanças individuais.
- O chefe pode consultar os dados financeiros individuais dos membros ativos da família, de acordo com as regras de autorização.
- Membros comuns veem seus próprios dados e os indicadores agregados de saúde financeira familiar.
- A saúde familiar permanece disponível mesmo quando a família não tem chefe.

### 3.9 Saúde financeira e painéis

- Saúde financeira individual e familiar são indicadores dinâmicos, não rótulos permanentes definidos manualmente.
- A avaliação pode considerar, entre outros fatores, a proporção de saídas em relação às entradas e a evolução da capacidade de poupar.
- Os rótulos ilustrativos discutidos incluem “se recuperando”, “sentindo sintomas ruins”, “dor de cabeça”, “enxaqueca pesada” e “saúde de ferro”. Os critérios, limiares e texto definitivo precisam ser definidos antes de implementar o cálculo.
- Os painéis devem apresentar dados históricos, mês atual e expectativa do mês seguinte.
- A expectativa inicial do mês seguinte usa compromissos conhecidos: parcelas futuras, ocorrências recorrentes previstas e demais compromissos confirmados, sem contar a mesma obrigação duas vezes.
- A projeção não deve ser apresentada como previsão garantida nem incluir estimativa por IA no MVP.

## 4. Requisitos funcionais

| ID | Requisito |
|---|---|
| RF01 | O sistema deve permitir cadastro, autenticação e encerramento de sessão do usuário. |
| RF02 | O sistema deve permitir ao usuário alterar sua senha após autenticação. |
| RF03 | O sistema deve confirmar o novo endereço antes de concluir uma alteração de e-mail. |
| RF04 | O sistema deve disponibilizar categorias predefinidas e permitir criar categorias próprias. |
| RF05 | O sistema deve permitir desativar categorias próprias preservando referências históricas. |
| RF06 | O sistema deve permitir cadastrar, consultar, editar e excluir entradas conforme regras de autorização e integridade. |
| RF07 | O sistema deve permitir cadastrar compras e dividir seus pagamentos entre meios e cartões diferentes. |
| RF08 | O sistema deve validar que a soma dos pagamentos de uma compra corresponde ao valor total da compra. |
| RF09 | O sistema deve gerar parcelas para pagamentos no crédito, inclusive crédito em uma única parcela. |
| RF10 | O sistema deve permitir cadastrar cartões e vincular usuários autorizados. |
| RF11 | O sistema deve permitir revogar a autorização de uso de um cartão sem apagar compromissos existentes. |
| RF12 | O sistema deve organizar parcelas em faturas de cartão. |
| RF13 | O sistema deve permitir registrar pagamentos integrais ou parciais de faturas. |
| RF14 | O sistema deve calcular total da fatura, total pago e saldo restante com base nos registros relacionados. |
| RF15 | O sistema deve permitir cadastrar despesas recorrentes e gerar ocorrências para os próximos seis meses. |
| RF16 | O sistema deve permitir pagar ou cancelar uma ocorrência de despesa recorrente. |
| RF17 | O sistema deve preservar o valor previsto de cada ocorrência para manter o histórico correto. |
| RF18 | O sistema deve permitir cadastrar investimentos e registrar aplicações e resgates. |
| RF19 | O sistema deve impedir resgates superiores ao saldo líquido registrado de um investimento. |
| RF20 | O sistema deve permitir criar família, gerar/renovar código e solicitar ingresso em uma família existente. |
| RF21 | O sistema deve permitir que administradores ativos aprovem ou recusem solicitações de entrada. |
| RF22 | O sistema deve preservar o histórico de participação em famílias e permitir saída ou remoção sem apagar os dados pessoais/financeiros. |
| RF23 | O sistema deve aplicar funções e permissões associadas à participação do usuário na família. |
| RF24 | O sistema deve permitir que administradores gerenciem funções, respeitando a manutenção de ao menos um administrador ativo. |
| RF25 | O sistema deve apresentar saúde financeira individual e familiar. |
| RF26 | O sistema deve apresentar histórico, mês atual e expectativa do mês seguinte com base em compromissos conhecidos. |
| RF27 | O sistema deve impedir acesso a dados de outros usuários sem autorização explícita pelas regras do domínio. |

## 5. Requisitos não funcionais

| ID | Requisito |
|---|---|
| RNF01 | A interface web deve ser responsiva para computador e dispositivos móveis. |
| RNF02 | O frontend deve ser desenvolvido com React e TypeScript. |
| RNF03 | O backend deve usar Node.js, TypeScript e Express. |
| RNF04 | A persistência principal deve usar MySQL e modelo relacional. |
| RNF05 | A comunicação entre frontend e backend deve ocorrer por API REST. |
| RNF06 | Em produção, o tráfego deve usar HTTPS. |
| RNF07 | Senhas devem ser armazenadas com algoritmo de hash apropriado, como Argon2id ou bcrypt, com parâmetros adequados. |
| RNF08 | Segredos, chaves e credenciais não devem ser versionados no repositório. |
| RNF09 | A base de dados não deve ser exposta diretamente à internet; o acesso deve ser limitado ao backend e a operadores autorizados. |
| RNF10 | Dados persistidos e backups devem ser protegidos por controles de acesso e criptografia apropriados. |
| RNF11 | O backend deve validar entradas, propriedade dos dados, permissões e transições de estado. |
| RNF12 | Operações que envolvem gravações relacionadas devem usar transações e rollback em caso de falha. |
| RNF13 | Valores monetários devem usar tipo decimal adequado no banco, nunca FLOAT/DOUBLE para cálculos financeiros. |
| RNF14 | Totais derivados devem ser calculados no backend ou na consulta apropriada; o frontend não pode impor seus próprios totais como verdade. |
| RNF15 | O banco deve usar restrições, chaves estrangeiras, índices e unicidade quando aplicáveis, complementadas por validação no backend. |
| RNF16 | O sistema deve manter histórico financeiro e de participação sem apagar registros necessários à auditoria e à compreensão do passado. |

## 6. Regras de segurança e autorização

- Toda operação protegida deve identificar o usuário autenticado no backend.
- O backend deve conferir a propriedade do registro e a permissão antes de ler, criar, alterar ou remover dados.
- O frontend não é uma fronteira de segurança; esconder um botão não substitui autorização no servidor.
- Ser administrador de família não concede automaticamente acesso a entradas, compras, investimentos ou dados financeiros pessoais dos demais membros.
- A permissão de chefe para consultar dados individuais deve ser verificada considerando a participação ativa e a família correta.
- A autorização de uso de cartão deve ser validada no momento de criar novo compromisso no crédito.
- O código de família não pode ser usado como prova de autorização para consultar dados.
- Erros e logs não devem expor senhas, tokens, segredos ou dados financeiros desnecessários.
- Alterações compostas — compra com pagamentos e parcelas, aprovação de entrada com criação de participação, pagamento de ocorrência com parcelas/fatura — devem ser atômicas.

## 7. Critérios de aceitação essenciais

1. Uma compra dividida em dois cartões só é confirmada quando a soma das partes de pagamento corresponde ao total da compra.
2. Uma parte paga no crédito gera as parcelas correspondentes; uma parte paga por PIX/dinheiro/débito não gera parcelas.
3. O valor de uma compra não é contado novamente por causa de suas parcelas.
4. Uma parcela tem uma única origem e uma única fatura.
5. Um usuário não consegue usar um cartão sem vínculo de autorização ativo.
6. Revogar autorização de cartão não apaga parcelas nem compras anteriores.
7. Pagar parcialmente uma fatura reduz seu saldo restante sem apagar o histórico dos pagamentos.
8. Uma recorrência ativa mantém ocorrências futuras dentro da janela móvel de seis meses sem duplicação.
9. Alterar o valor da recorrência não modifica ocorrências passadas nem já pagas.
10. Um resgate não pode exceder o saldo líquido de aplicações e resgates já registrados.
11. Um usuário não pode ter duas participações familiares ativas simultaneamente.
12. Aprovação de solicitação e criação de participação são atômicas.
13. Uma família não pode ficar sem administrador ativo; pode ficar sem chefe.
14. Sair de uma família preserva participação histórica e dados financeiros.
15. Um administrador comum não consegue ler finanças individuais de outro membro apenas por ser administrador.
16. Um membro comum consegue consultar indicadores agregados de saúde familiar sem receber acesso aos registros financeiros individuais de todos.
17. Totais da fatura são derivados dos registros relacionados e não aceitam edição direta pelo cliente.

## 8. Fora do escopo do MVP

- Integração Open Banking/Open Finance e importação automática de transações.
- Previsão de gastos com IA.
- Integração direta com bancos, corretoras ou provedores de investimento.
- Execução real de pagamentos ou transferências via PIX.
- Cálculo automático de juros, multas, tarifas e encargos de cartão.
- Avaliação de investimentos por cotação de mercado, rendimento, lucro ou prejuízo.
- Contas bancárias separadas e distribuição do caixa por banco/conta.
- Aplicativo nativo para Android/iOS; a primeira entrega é web responsiva.
- Metas, orçamentos avançados e alertas, salvo se forem aprovados como requisitos posteriores.

## 9. Decisões pendentes — não presumir como decididas

1. Permissões e transições exatas para cancelar uma solicitação pendente de entrada (solicitante, administrador, ou ambos).
2. Se o criador recebe também a função `CRIADOR`, além de `ADMINISTRADOR`.
3. Política para conflito/auto-revogação de administradores e tratamento de casos em que um administrador tente remover outro.
4. Valores válidos de `INVESTIMENTO.status` e se esse campo é realmente necessário no MVP.
5. Valores exatos de `USUARIO_CARTAO.status` e regras de transição.
6. Quem pode visualizar compras, parcelas e faturas de cartão: titular, usuário que lançou a compra e outros usuários autorizados, especialmente quando estão em famílias diferentes.
7. Regras exatas para ciclo de fechamento e alocação de parcelas em faturas, inclusive compras na data de fechamento e meses com dias inexistentes.
8. Política para pagamentos de fatura acima do saldo, estornos, chargebacks e correções.
9. Unicidade de nomes de categoria por usuário e por categorias predefinidas.
10. Critérios, pesos, limiares e textos definitivos para saúde financeira individual/familiar.
11. Fuso horário, regras de calendário e cálculo de vencimentos para recorrências semanais, mensais e anuais.
12. Regra exata para associar pagamentos de ocorrência no crédito à data da compra/lançamento e ao ciclo da fatura.
13. Política de exclusão/retificação de lançamentos já usados em parcelas, faturas ou indicadores.

## 10. Nota de implementação

Este documento consolida decisões de domínio conhecidas, mas não substitui a definição física final do banco nem testes de integridade. As pendências da seção 9 devem permanecer explícitas até serem decididas; não devem ser preenchidas silenciosamente por suposição.
