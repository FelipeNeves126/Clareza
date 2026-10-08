# Modelo de Domínio — Clareza

## 1. Objetivo

Este documento define o modelo conceitual do domínio do Clareza.

Seu objetivo é identificar as principais entidades do sistema, suas responsabilidades, relacionamentos e regras de associação antes da criação do modelo do banco de dados.

Este documento é derivado dos requisitos funcionais e das regras de negócio definidos anteriormente.

A estrutura aqui apresentada é conceitual. Os nomes de tabelas, tipos de dados, chaves estrangeiras e detalhes específicos do MySQL serão definidos posteriormente durante a modelagem do banco de dados.

---

# 2. Visão geral do domínio

O Clareza é organizado em quatro grandes áreas:

### Usuários e famílias

Responsável por:

* usuários;
* famílias;
* participação dos usuários nas famílias;
* funções e permissões;
* solicitações de entrada.

### Finanças

Responsável por:

* entradas;
* investimentos;
* compras;
* parcelas;
* despesas recorrentes;
* categorias;
* cartões.

### Análises

Responsável por:

* saúde financeira individual;
* saúde financeira da família;
* histórico financeiro;
* projeções.

### Segurança

Responsável por:

* autenticação;
* autorização;
* isolamento dos dados financeiros;
* controle de acesso às informações familiares.

---

# 3. Entidades principais

As entidades inicialmente identificadas são:

```text
USUARIO
FAMILIA
MEMBRO
FUNCAO
SOLICITACAO_ENTRADA

CATEGORIA

ENTRADA
INVESTIMENTO

COMPRA
PARCELA
CARTAO

DESPESA_RECORRENTE
```

O conceito de caixa financeiro consolidado também existe no domínio, mas não será inicialmente tratado como uma conta bancária individualizada.

---

# 4. USUARIO

## Responsabilidade

Representa uma pessoa que possui uma conta no Clareza.

O usuário pode utilizar o sistema individualmente ou participar de uma família.

Um usuário não precisa obrigatoriamente pertencer a uma família.

## Atributos conceituais

```text
id
nome
data_nascimento
data_cadastro
email
senha
```

## Observações

A idade não será armazenada diretamente.

O sistema poderá calcular a idade a partir da `data_nascimento`.

A saúde financeira atual não será armazenada como um atributo simples do usuário, pois ela deve ser calculada a partir dos dados financeiros.

O vínculo com uma família será representado por `MEMBRO`, e não diretamente por um campo `id_familia` em `USUARIO`.

---

# 5. FAMILIA

## Responsabilidade

Representa um grupo de usuários que decidiu compartilhar determinadas informações e funcionalidades dentro do Clareza.

## Atributos conceituais

```text
id
nome
codigo
data_criacao
```

## Observações

O código da família deve ser único.

Uma família pode existir sem nenhum chefe.

Uma família pode possuir vários membros.

A saúde financeira da família não será armazenada como um valor fixo.

Ela será calculada a partir das informações financeiras agregadas dos membros.

---

# 6. MEMBRO

## Responsabilidade

Representa a participação de um usuário em uma família.

Essa entidade existe porque o relacionamento entre usuário e família possui informações próprias e regras específicas.

## Atributos conceituais

```text
id
id_usuario
id_familia
data_entrada
```

## Regras

* Um usuário pode não possuir nenhum vínculo com uma família.
* Um usuário pode pertencer a no máximo uma família simultaneamente.
* Uma família pode possuir vários membros.
* Sair da família encerra o vínculo atual do usuário com aquela família.
* O histórico do vínculo poderá ser preservado futuramente caso o sistema necessite registrar participações anteriores.

## Observação importante

`MEMBRO` não representa necessariamente uma pessoa diferente de `USUARIO`.

Um membro é um usuário exercendo participação dentro de uma determinada família.

Conceitualmente:

```text
USUARIO
   │
   │ participa
   ▼
MEMBRO
   │
   │ pertence a
   ▼
FAMILIA
```

---

# 7. FUNCAO

## Responsabilidade

Representa uma função que um membro pode exercer dentro de uma família.

As funções inicialmente previstas são:

```text
CRIADOR
ADMINISTRADOR
CHEFE
MEMBRO
```

## Observação

As funções não devem ser tratadas como sinônimos de permissões.

Uma função representa uma responsabilidade ou papel.

A permissão determina o que aquele papel pode realizar.

Um usuário pode possuir mais de uma função simultaneamente.

Exemplo:

```text
Usuário Felipe
    ├── ADMINISTRADOR
    └── CHEFE
```

---

# 8. Relação entre MEMBRO e FUNCAO

Como um membro pode possuir mais de uma função, não devemos colocar simplesmente:

```text
MEMBRO
tipo = "administrador"
```

Isso impediria que o mesmo membro tivesse simultaneamente outras funções.

O relacionamento conceitual será:

```text
MEMBRO
   │
   │ possui
   ▼
FUNCAO
```

Esse relacionamento deverá permitir:

```text
Membro A
    ├── ADMINISTRADOR
    └── CHEFE

Membro B
    └── MEMBRO

Membro C
    └── ADMINISTRADOR
```

No modelo físico do banco, esse relacionamento provavelmente será representado por uma estrutura associativa.

---

# 9. SOLICITACAO_ENTRADA

## Responsabilidade

Representa o pedido de um usuário para participar de uma família.

## Atributos conceituais

```text
id
id_usuario
id_familia
data_solicitacao
status
```

## Status

Inicialmente:

```text
PENDENTE
APROVADA
REJEITADA
```

## Regras

* A solicitação pertence a um usuário.
* A solicitação é direcionada a uma família.
* Uma solicitação pendente não torna o usuário membro.
* Apenas um administrador da família pode aprovar ou rejeitar a solicitação.
* Não devem existir duas solicitações pendentes simultâneas do mesmo usuário para a mesma família.
* Quando aprovada, a solicitação resulta na criação do vínculo do usuário com a família.

Conceitualmente:

```text
USUARIO
   │
   │ solicita entrada
   ▼
SOLICITACAO_ENTRADA
   │
   │ para
   ▼
FAMILIA
```

---

# 10. CATEGORIA

## Responsabilidade

Representa uma classificação utilizada para organizar informações financeiras.

## Atributos conceituais

```text
id
nome
```

Possivelmente poderão existir atributos adicionais futuramente, como:

```text
tipo
id_usuario
```

para diferenciar categorias do sistema e categorias personalizadas.

Essa decisão será fechada durante a modelagem física.

## Exemplos

```text
Alimentação
Transporte
Moradia
Salário
Lazer
Educação
Investimentos
Saúde
```

## Regras

* O usuário poderá utilizar categorias predefinidas.
* O usuário poderá criar categorias personalizadas.
* Uma categoria personalizada criada por um usuário não deve alterar automaticamente as categorias de outros usuários.

---

# 11. ENTRADA

## Responsabilidade

Representa um valor recebido pelo usuário.

## Atributos conceituais

```text
id
id_usuario
id_categoria
data
valor
descricao
```

## Exemplos

```text
Salário
Freelance
Venda
Renda extra
Presente
```

Uma entrada aumenta os recursos financeiros disponíveis do usuário.

---

# 12. INVESTIMENTO

## Responsabilidade

Representa um valor destinado a uma aplicação ou investimento financeiro.

## Atributos conceituais

```text
id
id_usuario
id_categoria
data
valor
descricao
```

## Regra

Um investimento não deve ser classificado como despesa.

Para a primeira versão, o controle dos investimentos será manual.

O sistema não realizará automaticamente:

* compra de ativos;
* venda de ativos;
* integração com corretoras;
* cálculo automático de rentabilidade;
* atualização automática de cotação.

Essas funcionalidades poderão ser adicionadas futuramente.

---

# 13. COMPRA

## Responsabilidade

Representa uma compra realizada pelo usuário.

Uma compra pode ser realizada à vista ou parcelada.

## Atributos conceituais

```text
id
id_usuario
id_categoria
data_compra
valor_total
forma_pagamento
condicao_pagamento
id_cartao
```

## Forma de pagamento

A forma de pagamento representa como a compra foi realizada.

Exemplos:

```text
DINHEIRO
PIX
DEBITO
CREDITO
```

## Condição de pagamento

A condição representa se a compra foi realizada em uma ou várias partes.

Exemplos:

```text
A_VISTA
PARCELADA
```

## Regras

Uma compra à vista possui pagamento único.

Uma compra parcelada possui uma quantidade determinada de parcelas.

A forma de pagamento e a condição de pagamento são conceitos diferentes.

Exemplo:

```text
Compra de R$ 100 via PIX

forma_pagamento = PIX
condicao_pagamento = A_VISTA
```

Exemplo:

```text
Compra de R$ 1.000 no cartão em 10 vezes

forma_pagamento = CREDITO
condicao_pagamento = PARCELADA
```

O cartão só deverá ser associado à compra quando a forma de pagamento exigir cartão.

---

# 14. PARCELA

## Responsabilidade

Representa uma parcela pertencente a uma compra parcelada.

## Atributos conceituais

```text
id
id_compra
numero
valor
data_vencimento
status
```

## Status

Inicialmente poderão existir estados como:

```text
PENDENTE
PAGA
```

Outros estados poderão ser adicionados futuramente caso sejam necessários.

## Regras

* Uma parcela pertence a uma única compra.
* Uma compra pode possuir várias parcelas.
* As parcelas representam partes da mesma compra.
* Uma parcela não representa uma nova compra.
* Uma compra parcelada possui uma quantidade definida de parcelas.
* O parcelamento termina quando todas as parcelas previstas forem concluídas.

Exemplo:

```text
COMPRA
Notebook
R$ 3.000
10 parcelas

        │
        ├── Parcela 1 — R$ 300
        ├── Parcela 2 — R$ 300
        ├── Parcela 3 — R$ 300
        ├── ...
        └── Parcela 10 — R$ 300
```

---

# 15. CARTAO

## Responsabilidade

Representa um cartão de crédito utilizado pelo usuário para realizar compras.

## Atributos conceituais

```text
id
id_usuario
nome
limite_total
dia_fechamento
dia_vencimento
```

## Regras

* Um cartão pertence a um usuário.
* Um usuário poderá possuir mais de um cartão.
* Uma compra pode utilizar um cartão.
* O cartão só deve ser associado a compras realizadas por crédito.
* O limite utilizado não será inicialmente armazenado como valor independente.
* O limite utilizado poderá ser calculado a partir das compras e parcelas que comprometem o limite.

## Observação

Não armazenar inicialmente `limite_utilizado` evita manter duas fontes de verdade.

Exemplo:

```text
limite_total = R$ 5.000

Compras que comprometem o limite:
R$ 500
R$ 1.000
R$ 300

Limite utilizado = R$ 1.800
Limite disponível = R$ 3.200
```

Esses valores podem ser derivados dos registros financeiros.

---

# 16. DESPESA_RECORRENTE

## Responsabilidade

Representa uma despesa que se repete ao longo do tempo enquanto estiver ativa.

## Exemplos

```text
Aluguel
Internet
Netflix
Academia
Mensalidade
```

## Atributos conceituais

```text
id
id_usuario
id_categoria
descricao
valor
data_inicio
data_fim
periodicidade
status
```

## Status

Inicialmente:

```text
ATIVA
ENCERRADA
```

## Regras

* Uma despesa recorrente continua gerando compromissos enquanto estiver ativa.
* O usuário pode encerrá-la.
* O encerramento impede novas ocorrências futuras.
* Ocorrências já registradas não devem ser apagadas.
* Despesa recorrente não deve ser confundida com compra parcelada.

---

# 17. Diferença entre COMPRA, PARCELA e DESPESA_RECORRENTE

Esses três conceitos precisam permanecer separados.

## Compra à vista

```text
COMPRA
R$ 500
A_VISTA
```

Existe uma compra e seu pagamento ocorre de uma vez.

## Compra parcelada

```text
COMPRA
R$ 1.000
PARCELADA

    ├── PARCELA 1
    ├── PARCELA 2
    ├── PARCELA 3
    └── PARCELA 4
```

A quantidade de parcelas é definida no momento da compra.

## Despesa recorrente

```text
DESPESA_RECORRENTE
Internet
R$ 100
ATIVA

    ├── ocorrência de outubro
    ├── ocorrência de novembro
    ├── ocorrência de dezembro
    └── ...
```

A recorrência continua até ser encerrada.

---

# 18. Caixa financeiro consolidado

## Conceito

Na primeira versão do Clareza, o dinheiro do usuário será tratado de maneira consolidada.

O sistema não terá inicialmente uma estrutura detalhada para separar:

```text
Banco A
Banco B
Carteira
Conta corrente
Conta poupança
```

O objetivo inicial é permitir que o sistema controle a situação financeira geral do usuário.

## Consequência

O modelo inicial não precisa representar contas bancárias individuais.

O sistema poderá calcular informações como:

```text
Entradas
-
Despesas
-
Investimentos
=
Resultado financeiro
```

ou outros indicadores derivados conforme as regras de cálculo forem definidas.

## Evolução futura

A integração com Open Banking poderá introduzir posteriormente conceitos como:

```text
CONTA_BANCARIA
INSTITUICAO_FINANCEIRA
SALDO
TRANSACAO_BANCARIA
CONTA_CARTAO
FATURA
```

Esses conceitos não fazem parte do modelo inicial.

---

# 19. Saúde financeira

A saúde financeira não será inicialmente tratada como uma entidade financeira independente.

Ela será um indicador calculado a partir dos dados existentes.

## Saúde individual

Considerará fatores como:

```text
Entradas
Despesas
Investimentos
Capacidade de economia
Despesas recorrentes
Parcelas
Compromissos futuros
Distribuição dos gastos
Evolução histórica
```

## Saúde familiar

Será calculada a partir de informações agregadas dos membros da família.

## Estados possíveis

```text
Saúde de ferro
Saudável
Se recuperando
Sentindo uma dor de cabeça
Sentindo sintomas ruins
Enxaqueca pesada
```

## Importante

Esses estados são uma representação da análise do sistema.

Eles não devem ser armazenados inicialmente como:

```text
usuario.saude = "Saudável"
```

A classificação deverá ser obtida a partir dos dados financeiros.

---

# 20. Projeção financeira

O sistema terá dois conceitos diferentes.

## Projeção confirmada

Utiliza informações já conhecidas e registradas.

Exemplos:

```text
Parcelas futuras
Despesas recorrentes ativas
Outros compromissos registrados
```

## Previsão inteligente

Representa uma estimativa baseada em histórico e comportamento.

Exemplos de dados que poderão ser utilizados futuramente:

```text
Histórico de gastos
Categorias
Recorrências
Parcelamentos
Comportamento de consumo
Sazonalidade
```

A previsão inteligente não faz parte da primeira versão.

---

# 21. Relacionamentos principais

## USUARIO → MEMBRO

Um usuário pode possuir:

```text
0 ou 1 participação familiar ativa
```

Um membro representa um usuário dentro de uma família.

---

## FAMILIA → MEMBRO

Uma família pode possuir:

```text
0 ou vários membros
```

---

## MEMBRO → FUNCAO

Um membro pode possuir:

```text
1 ou várias funções
```

Uma função pode ser atribuída a vários membros.

---

## USUARIO → SOLICITACAO_ENTRADA

Um usuário pode realizar várias solicitações ao longo do tempo.

Cada solicitação pertence a um único usuário.

---

## FAMILIA → SOLICITACAO_ENTRADA

Uma família pode receber várias solicitações.

Cada solicitação é destinada a uma única família.

---

## USUARIO → ENTRADA

Um usuário pode possuir várias entradas.

Cada entrada pertence a um único usuário.

---

## USUARIO → INVESTIMENTO

Um usuário pode possuir vários investimentos.

Cada investimento pertence a um único usuário.

---

## USUARIO → COMPRA

Um usuário pode realizar várias compras.

Cada compra pertence a um único usuário.

---

## COMPRA → PARCELA

Uma compra pode possuir:

```text
1 parcela
```

quando realizada à vista, conceitualmente, ou várias parcelas quando parcelada.

Na implementação, podemos optar por registrar parcelas apenas para compras parceladas.

Uma compra parcelada possuirá uma quantidade definida de parcelas.

Cada parcela pertence a uma única compra.

---

## USUARIO → CARTAO

Um usuário pode possuir vários cartões.

Cada cartão pertence a um único usuário.

---

## CARTAO → COMPRA

Um cartão pode ser utilizado em várias compras.

Uma compra pode estar associada a um cartão quando realizada por crédito.

---

## USUARIO → DESPESA_RECORRENTE

Um usuário pode possuir várias despesas recorrentes.

Cada despesa recorrente pertence a um único usuário.

---

## CATEGORIA → REGISTROS_FINANCEIROS

Uma categoria pode ser utilizada por vários registros financeiros.

Um registro financeiro pode possuir uma categoria.

A forma exata desse relacionamento será refinada no modelo físico.

---

# 22. Visão conceitual simplificada

A estrutura principal pode ser visualizada da seguinte forma:

```text
                         ┌──────────────┐
                         │    FAMILIA   │
                         └──────┬───────┘
                                │
                                │
                         ┌──────▼───────┐
                         │    MEMBRO    │
                         └──────┬───────┘
                                │
                         ┌──────▼───────┐
                         │    USUARIO   │
                         └──────┬───────┘
                                │
          ┌─────────────────────┼─────────────────────┐
          │                     │                     │
          ▼                     ▼                     ▼
      ENTRADA              INVESTIMENTO            COMPRA
                                                       │
                                                       ▼
                                                    PARCELA
                                                       │
                                                       │
                                                    CARTAO


USUARIO ─────────────── DESPESA_RECORRENTE

USUARIO ─────────────── SOLICITACAO_ENTRADA ───────── FAMILIA

MEMBRO ──────────────── FUNCAO

REGISTROS FINANCEIROS ───────── CATEGORIA
```

---

# 23. Entidades que não serão criadas inicialmente

Para evitar complexidade desnecessária, as seguintes entidades ficam fora da primeira versão:

```text
CONTA_BANCARIA
BANCO
INSTITUICAO_FINANCEIRA
FATURA
TRANSACAO_BANCARIA
SALDO_BANCARIO
CORRETORA
ATIVO
```

Esses conceitos poderão ser introduzidos posteriormente, principalmente durante a implementação de Open Banking.

Também não será criada inicialmente uma entidade específica para:

```text
SAIDA
```

porque as diferentes formas de despesa já possuem representações mais específicas:

```text
COMPRA
DESPESA_RECORRENTE
PARCELA
```

---

# 24. Decisões de modelagem atuais

As decisões atuais do domínio são:

### Família

* Usuário pode existir sem família.
* Usuário pode pertencer a apenas uma família simultaneamente.
* Família pode ter vários membros.
* Família pode existir sem chefe.
* Família possui código único.
* Entrada na família exige solicitação.

### Funções

* Criador, administrador, chefe e membro são conceitos distintos.
* Um usuário pode possuir múltiplas funções.
* Chefe não é automaticamente administrador.
* Administrador não possui automaticamente acesso aos dados financeiros individuais.
* Chefe pode visualizar dados individuais dos membros da própria família.

### Financeiro

* Movimentações principais: entrada, despesa e investimento.
* Investimento não é despesa.
* Compra é uma forma específica de despesa.
* Compra pode ser à vista ou parcelada.
* Parcelamento possui quantidade definida.
* Recorrência é diferente de parcelamento.
* Cartão de crédito é separado da compra.
* Categorias são entidades próprias.
* O caixa será inicialmente consolidado.

### Projeção

* Dados passados utilizam valores registrados.
* Mês atual pode apresentar valores realizados e compromissos conhecidos.
* Períodos futuros podem utilizar compromissos conhecidos.
* Projeção confirmada não é previsão inteligente.
* Previsão inteligente será implementada futuramente.

---

# 25. Pontos que ainda precisam ser definidos

Antes da criação do banco de dados, ainda precisamos definir:

1. Como exatamente uma compra à vista será registrada em relação às parcelas.
2. Se todas as compras terão um registro de parcela ou somente as parceladas.
3. Como uma parcela será considerada paga.
4. Como o pagamento da fatura do cartão será representado.
5. Como o limite comprometido do cartão será calculado.
6. Como as despesas recorrentes gerarão suas ocorrências.
7. Como será calculado o saldo consolidado do usuário.
8. Como investimentos afetarão o caixa consolidado.
9. Como categorias personalizadas serão relacionadas aos usuários.
10. Como as funções serão armazenadas no banco.
11. Como a função de criador será tratada após mudanças de administradores.
12. Como será calculada matematicamente a saúde financeira.
13. Quais informações agregadas da família serão exibidas para membros comuns.
14. Como o histórico financeiro será tratado em exclusões e alterações.
15. Como será feita a distinção entre despesa realizada e compromisso futuro.

Essas decisões devem ser fechadas antes da criação definitiva das tabelas.

---

# 26. Próxima etapa

Depois que este modelo conceitual for validado, o projeto deverá avançar para:

```text
Modelo de domínio
        ↓
Atributos definitivos
        ↓
Relacionamentos definitivos
        ↓
Cardinalidades
        ↓
DER
        ↓
Modelo relacional
        ↓
SQL / MySQL
```

Não devemos criar o banco de dados antes dessa etapa estar suficientemente definida, pois mudanças no domínio podem exigir alterações estruturais no banco.
