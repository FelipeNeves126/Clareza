# Clareza — Modelo de Domínio

**Versão:** 1.3  
**Objetivo:** consolidar entidades, atributos, relacionamentos e restrições de domínio conhecidas. Este documento é conceitual/lógico e não substitui o DDL físico final.

## 1. Convenções

- `PK`: chave primária.
- `FK`: chave estrangeira.
- `UNIQUE`: valor ou combinação única.
- `NULL`: atributo opcional.
- Valores monetários devem usar tipo decimal apropriado no MySQL, por exemplo `DECIMAL`, com precisão definida na implementação.
- Datas de eventos financeiros usam data de calendário quando não houver necessidade explícita de horário.
- A identidade do proprietário deve ser derivada do usuário autenticado no backend.
- Nomes de campos abaixo são propostas de domínio; a convenção final de nomes deve ser aplicada consistentemente no banco e na API.

## 2. Entidades

### 2.1 USUARIO

| Atributo | Regra |
|---|---|
| `id` | PK |
| `nome` | Obrigatório |
| `data_nascimento` | Obrigatória ou opcional conforme validação de cadastro a definir |
| `data_cadastro` | Obrigatória; definida pelo sistema |
| `email` | Obrigatório e UNIQUE |
| `senha_hash` | Obrigatório; hash seguro, nunca senha em texto puro |

Não manter `id_familia`, `id_membro` nem saúde financeira estática diretamente em `USUARIO`. A participação familiar é histórica e a saúde é calculada dinamicamente.

### 2.2 FAMILIA

| Atributo | Regra |
|---|---|
| `id` | PK |
| `nome` | Obrigatório; não precisa ser único |
| `codigo` | Obrigatório, UNIQUE, aleatório/difícil de adivinhar |
| `data_criacao` | Definida pelo sistema |

O código pode ser regenerado. A regeneração invalida imediatamente o código anterior, sem excluir membros ou cancelar automaticamente solicitações já existentes.

### 2.3 MEMBRO

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_usuario` | FK → `USUARIO.id` |
| `id_familia` | FK → `FAMILIA.id` |
| `data_entrada` | Obrigatória |
| `data_saida` | NULL enquanto ativo; preenchida ao encerrar |

`data_saida IS NULL` representa participação ativa. Um usuário pode ter várias participações históricas, mas no máximo uma ativa simultaneamente. Reingresso cria novo registro.

### 2.4 FUNCAO

| Atributo | Regra |
|---|---|
| `id` | PK |
| `nome` | UNIQUE; valores previstos: `CRIADOR`, `ADMINISTRADOR`, `CHEFE`, `MEMBRO` |

As funções são dados de domínio/semente. A atribuição adicional de `CRIADOR` ao criador da família permanece pendente; `ADMINISTRADOR` é atribuído automaticamente.

### 2.5 MEMBRO_FUNCAO

| Atributo | Regra |
|---|---|
| `id_membro` | FK → `MEMBRO.id` |
| `id_funcao` | FK → `FUNCAO.id` |

Chave primária composta ou UNIQUE (`id_membro`, `id_funcao`). A função está vinculada à participação, não globalmente ao usuário. Deve-se impedir múltiplos chefes ativos na mesma família e garantir ao menos um administrador ativo.

### 2.6 SOLICITACAO_ENTRADA

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_usuario` | FK → `USUARIO.id` |
| `id_familia` | FK → `FAMILIA.id` |
| `data_solicitacao` | Definida pelo sistema |
| `status` | `PENDENTE`, `APROVADA`, `RECUSADA`, `CANCELADA` |

Pode haver várias solicitações históricas do mesmo usuário para a mesma família, mas não mais de uma pendente simultaneamente. A política de cancelamento de solicitação pendente está pendente de decisão.

### 2.7 CATEGORIA

| Atributo | Regra |
|---|---|
| `id` | PK |
| `nome` | Obrigatório |
| `id_usuario` | FK → `USUARIO.id`, NULL para categoria predefinida |
| `ativa` | Booleano; categorias inativas preservam histórico |

A unicidade do nome precisa ser definida considerando categorias predefinidas e categorias próprias. Categoria inativa não pode ser usada em novos registros. O usuário não pode editar/excluir categoria predefinida.

### 2.8 ENTRADA

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_usuario` | FK → `USUARIO.id` |
| `id_categoria` | FK → `CATEGORIA.id` |
| `data_entrada` | Data de calendário |
| `valor` | Decimal, maior que zero |
| `descricao` | Opcional |

Cada entrada pertence a um usuário e a uma categoria. O backend valida que a categoria seja predefinida ou pertença ao usuário.

### 2.9 INVESTIMENTO

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_usuario` | FK → `USUARIO.id` |
| `id_categoria` | FK → `CATEGORIA.id` |
| `nome` | Obrigatório |
| `descricao` | Opcional |
| `data_inicio` | Data de início |
| `status` | Conjunto de valores pendente de decisão |

O saldo não deve ser tratado como campo independente se puder ser derivado das movimentações. Se for armazenado por desempenho, deve ser reconciliável com as movimentações. A necessidade e os valores de `status` ainda precisam ser decididos.

### 2.10 MOVIMENTACAO_INVESTIMENTO

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_investimento` | FK → `INVESTIMENTO.id` |
| `tipo` | `APLICACAO` ou `RESGATE` |
| `data_movimentacao` | Data da movimentação |
| `valor` | Decimal, maior que zero |
| `descricao` | Opcional |

Saldo líquido registrado = soma de aplicações − soma de resgates. Resgate não pode exceder esse saldo. Não representa necessariamente valor de mercado.

### 2.11 CARTAO

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_titular` | FK → `USUARIO.id` |
| `nome` | Nome identificador |
| `limite_total` | Decimal positivo ou não negativo conforme regra final |
| `dia_fechamento` | Dia de fechamento, dentro de limites válidos |
| `dia_vencimento` | Dia de vencimento, dentro de limites válidos |

Um cartão tem um titular cadastrado no Clareza. Isso não valida titularidade legal com o emissor. O limite é compartilhado entre todos os usuários autorizados. Regras para meses curtos e fechamento ainda precisam ser formalizadas.

### 2.12 USUARIO_CARTAO

| Atributo | Regra |
|---|---|
| `id_usuario` | FK → `USUARIO.id` |
| `id_cartao` | FK → `CARTAO.id` |
| `data_vinculacao` | Data de autorização |
| `status` | Estados pendentes de decisão |

Usar PK composta ou UNIQUE (`id_usuario`, `id_cartao`). Revogar autorização impede novos compromissos, mas preserva os registros anteriores. Um usuário autorizado pode estar fora da família do titular. Os estados e transições exatos ainda precisam ser definidos.

### 2.13 COMPRA

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_usuario` | FK → `USUARIO.id` |
| `id_categoria` | FK → `CATEGORIA.id` |
| `descricao` | Obrigatória ou validada conforme política de UX |
| `data_compra` | Data de calendário |
| `valor_total` | Decimal maior que zero |

A compra não deve ter um único `id_cartao` ou uma única `forma_pagamento`, pois pode ser dividida entre vários meios e cartões. Não é necessário um campo genérico `condicao_pagamento` se as parcelas reais representam a condição de crédito.

### 2.14 PAGAMENTO_COMPRA

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_compra` | FK → `COMPRA.id` |
| `id_cartao` | FK → `CARTAO.id`, NULL para meios não creditícios |
| `forma_pagamento` | `PIX`, `DINHEIRO`, `DEBITO`, `CREDITO` |
| `valor` | Decimal maior que zero |

Cada linha representa uma parte do pagamento. A soma de todas as linhas deve ser igual a `COMPRA.valor_total`. Se `forma_pagamento = CREDITO`, `id_cartao` é obrigatório e deve haver autorização ativa. Para os demais meios, `id_cartao` deve ser NULL.

### 2.15 FATURA_CARTAO

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_cartao` | FK → `CARTAO.id` |
| `mes_referencia` | Mês/ano ou data normalizada de referência |
| `data_fechamento` | Data de fechamento |
| `data_vencimento` | Data de vencimento |

UNIQUE (`id_cartao`, `mes_referencia`). Cada fatura pertence a um cartão. `valor_total`, `total_pago` e `saldo_restante` devem ser calculados a partir de parcelas e pagamentos, não editados independentemente. A política de alocação de parcelas por ciclo ainda precisa ser definida.

### 2.16 PARCELA

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_pagamento_compra` | FK → `PAGAMENTO_COMPRA.id`, NULL se origem for recorrência |
| `id_ocorrencia_despesa` | FK → `OCORRENCIA_DESPESA.id`, NULL se origem for compra |
| `id_fatura` | FK → `FATURA_CARTAO.id` |
| `numero` | Número da parcela |
| `valor` | Decimal maior que zero |
| `data_vencimento` | Data de vencimento |

Cada parcela deve ter exatamente uma origem: `id_pagamento_compra` XOR `id_ocorrencia_despesa`. Não pode haver parcela sem origem nem com ambas. Cada parcela pertence a exatamente uma fatura. As parcelas de um pagamento de compra no crédito devem somar o valor daquela parte do pagamento. A unicidade do número da parcela por origem deve ser aplicada. Não existe status individual de parcela paga/pendente no modelo atual; a quitação é acompanhada pela fatura.

### 2.17 PAGAMENTO_FATURA

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_fatura` | FK → `FATURA_CARTAO.id` |
| `data_pagamento` | Data do pagamento |
| `valor` | Decimal maior que zero |

Uma fatura pode receber vários pagamentos. Total pago é a soma dos pagamentos relacionados. Política para sobrepagamento, estorno, chargeback e correção ainda está pendente.

### 2.18 DESPESA_RECORRENTE

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_usuario` | FK → `USUARIO.id` |
| `id_categoria` | FK → `CATEGORIA.id` |
| `descricao` | Obrigatória |
| `valor` | Decimal positivo para novas ocorrências |
| `periodicidade` | `SEMANAL`, `MENSAL`, `ANUAL` |
| `data_inicio` | Obrigatória |
| `data_fim` | Opcional |
| `status` | `ATIVA` ou `INATIVA` |

Recorrências ativas mantêm ocorrências geradas para os próximos seis meses. Desativar interrompe novas gerações e preserva histórico. Alteração de valor não modifica ocorrências passadas nem pagas.

### 2.19 OCORRENCIA_DESPESA

| Atributo | Regra |
|---|---|
| `id` | PK |
| `id_despesa_recorrente` | FK → `DESPESA_RECORRENTE.id` |
| `data_vencimento` | Data de vencimento |
| `valor_previsto` | Valor previsto preservado para histórico |
| `status` | `PENDENTE`, `PAGA`, `CANCELADA` |
| `data_pagamento` | NULL enquanto pendente, quando aplicável |
| `valor_pago` | NULL enquanto pendente, quando aplicável |
| `forma_pagamento` | NULL enquanto pendente; após pagar, forma registrada |
| `id_cartao` | NULL para meios não creditícios; obrigatório para crédito |

UNIQUE (`id_despesa_recorrente`, `data_vencimento`). Uma ocorrência pertence a uma única recorrência. No MVP, uma ocorrência utiliza uma única forma de pagamento. Crédito exige cartão autorizado e gera parcelas; PIX/dinheiro/débito não geram parcelas. A semântica de `data_pagamento` em uma ocorrência no crédito deve ser esclarecida: pode representar a data do lançamento no cartão, e não a quitação da fatura.

## 3. Relacionamentos e cardinalidades

| Nº | Relacionamento | Cardinalidade e significado |
|---|---|---|
| 1 | `USUARIO` — `MEMBRO` | Um usuário pode ter 0..N participações históricas; cada participação pertence a um usuário. |
| 2 | `FAMILIA` — `MEMBRO` | Uma família pode ter 0..N participações históricas; cada participação pertence a uma família. |
| 3 | `MEMBRO` — `MEMBRO_FUNCAO` | Uma participação pode ter 0..N atribuições de função. |
| 4 | `FUNCAO` — `MEMBRO_FUNCAO` | Uma função pode estar atribuída a 0..N participações. |
| 5 | `USUARIO` — `SOLICITACAO_ENTRADA` | Um usuário pode criar 0..N solicitações históricas. |
| 6 | `FAMILIA` — `SOLICITACAO_ENTRADA` | Uma família pode receber 0..N solicitações históricas. |
| 7 | `USUARIO` — `CATEGORIA` | Um usuário pode possuir 0..N categorias próprias; `id_usuario` nulo representa categoria predefinida. |
| 8 | `USUARIO` — `ENTRADA` | Um usuário pode ter 0..N entradas. |
| 9 | `CATEGORIA` — `ENTRADA` | Uma categoria pode classificar 0..N entradas. |
| 10 | `USUARIO` — `INVESTIMENTO` | Um usuário pode ter 0..N investimentos. |
| 11 | `CATEGORIA` — `INVESTIMENTO` | Uma categoria pode classificar 0..N investimentos. |
| 12 | `USUARIO` — `CARTAO` | Um usuário pode ser titular de 0..N cartões. |
| 13 | `USUARIO` — `CARTAO` via `USUARIO_CARTAO` | N:N: um usuário pode estar autorizado em vários cartões; um cartão pode ter vários usuários autorizados. |
| 14 | `USUARIO` — `COMPRA` | Um usuário pode registrar 0..N compras. |
| 15 | `CATEGORIA` — `COMPRA` | Uma categoria pode classificar 0..N compras. |
| 16 | `COMPRA` — `PAGAMENTO_COMPRA` | Uma compra confirmada tem 1..N partes de pagamento; cada parte pertence a uma compra. |
| 17 | `CARTAO` — `PAGAMENTO_COMPRA` | Um cartão pode ser usado em 0..N partes de pagamento no crédito; o cartão é NULL para meios não creditícios. |
| 18 | `PAGAMENTO_COMPRA` — `PARCELA` | Uma parte de pagamento no crédito pode gerar 1..N parcelas; meios não creditícios geram zero. |
| 19 | `USUARIO` — `DESPESA_RECORRENTE` | Um usuário pode ter 0..N configurações de despesas recorrentes. |
| 20 | `CATEGORIA` — `DESPESA_RECORRENTE` | Uma categoria pode classificar 0..N recorrências. |
| 21 | `DESPESA_RECORRENTE` — `OCORRENCIA_DESPESA` | Uma recorrência pode gerar 0..N ocorrências. |
| 22 | `OCORRENCIA_DESPESA` — `PARCELA` | Uma ocorrência paga no crédito pode gerar 1..N parcelas; sem crédito gera zero. |
| 23 | `CARTAO` — `FATURA_CARTAO` | Um cartão possui 0..N faturas; cada fatura pertence a um cartão. |
| 24 | `FATURA_CARTAO` — `PARCELA` | Uma fatura contém 0..N parcelas; cada parcela pertence a exatamente uma fatura. |
| 25 | `FATURA_CARTAO` — `PAGAMENTO_FATURA` | Uma fatura pode ter 0..N pagamentos; cada pagamento pertence a uma fatura. |
| 26 | `INVESTIMENTO` — `MOVIMENTACAO_INVESTIMENTO` | Um investimento pode ter 0..N movimentações; cada movimentação pertence a um investimento. |

## 4. Restrições de integridade transversais

1. Impedir mais de uma participação familiar ativa por usuário.
2. Impedir mais de uma solicitação pendente por usuário/família.
3. Manter ao menos um administrador ativo por família.
4. Permitir no máximo um chefe ativo por família.
5. Impedir uso de cartão sem autorização ativa.
6. Impedir cartão em pagamentos não creditícios.
7. Exigir cartão em pagamentos creditícios.
8. Validar a soma de `PAGAMENTO_COMPRA.valor` contra `COMPRA.valor_total`.
9. Validar a soma das parcelas contra a parte do pagamento no crédito que as originou.
10. Garantir que cada parcela tenha exatamente uma origem.
11. Impedir duplicação de ocorrência para recorrência e vencimento.
12. Impedir resgate superior ao saldo líquido registrado do investimento.
13. Calcular totais de fatura a partir das parcelas e pagamentos relacionados.
14. Executar operações compostas em transações.
15. Não apagar registros históricos apenas para simular saída de família, revogação de cartão ou desativação de categoria.
16. Conferir permissões no backend para cada operação, incluindo acesso a dados de família e cartões.

## 5. Notas de modelagem e implementação

- `PARCELA` não tem status individual `PENDENTE/PAGA` no modelo atual. A situação de pagamento é acompanhada pela fatura.
- `FATURA_CARTAO.valor_total`, `total_pago` e `saldo_restante` são atributos derivados, não fontes de verdade independentes.
- O valor da compra é a fonte de verdade para o gasto realizado; parcelas representam compromissos de fatura. Não somar ambos no mesmo indicador.
- O valor previsto da ocorrência é preservado mesmo quando o valor padrão da recorrência muda.
- Atribuição de parcela a fatura deve ser definida por regra de ciclo de cartão, incluindo compras próximas ao fechamento.
- `CARTAO.id_titular` é titularidade dentro do app, não confirmação de titularidade legal.
- Para garantir “pelo menos um administrador” e exclusividade de chefe, pode ser necessário combinar transações, bloqueios/isolamento apropriados e validações no backend, pois uma simples FK não assegura essas regras.
- Restrições como XOR de origem da parcela e valores positivos devem ser representadas por `CHECK` quando suportado pela versão do MySQL e validadas também na aplicação.
- A unicidade de categorias requer uma decisão de política antes de definir índice físico definitivo.

## 6. Decisões pendentes

1. Cancelamento de solicitação pendente: quem pode cancelar e quais transições são válidas.
2. Se o criador recebe `CRIADOR` além de `ADMINISTRADOR`.
3. Regras detalhadas para conflito entre administradores.
4. Valores válidos e finalidade de `INVESTIMENTO.status`.
5. Valores e transições de `USUARIO_CARTAO.status`.
6. Permissões de visualização de compras e faturas por titular, autor da compra e usuários autorizados.
7. Ciclo exato de fechamento/vencimento e alocação de parcelas, inclusive meses curtos.
8. Sobrepagamentos, estornos, chargebacks e correções de fatura.
9. Unicidade de categorias por proprietário e tratamento de categorias predefinidas.
10. Fórmula, pesos e faixas de saúde financeira.
11. Regras de calendário/fuso horário para recorrências.
12. Significado preciso de `OCORRENCIA_DESPESA.data_pagamento` quando o método é crédito.
13. Política de edição/exclusão de compras e ocorrências com parcelas existentes.
14. Política de arredondamento quando o valor de uma compra não se divide igualmente entre parcelas.
15. Definição final de obrigatoriedade e limites de alguns atributos, como data de nascimento, limite do cartão e descrição de compra.

## 7. Recomendação para o próximo passo

Antes de criar o DDL definitivo, fechar as decisões pendentes mais críticas para integridade e privacidade: visibilidade de cartão/fatura entre usuários autorizados, ciclo de fatura, cancelamento de solicitação e política de alterações de registros com parcelas. Em seguida, gerar o esquema MySQL com PK/FK/UNIQUE/CHECK/índices e testar as invariantes em cenários concorrentes.
