# Clareza — Documento de Requisitos

**Versão:** 1.2
**Projeto:** Clareza
**Tipo:** Projeto de estudo e portfólio

---

# 1. Visão do Projeto

O **Clareza** é um sistema web de controle financeiro pessoal e familiar.

O sistema permitirá que usuários registrem e acompanhem suas movimentações financeiras, como:

* Entradas;
* Despesas;
* Investimentos.

As informações poderão ser visualizadas por meio de:

* Dashboards;
* Tabelas;
* Gráficos;
* Indicadores financeiros;
* Projeções de gastos.

Além do controle individual, os usuários poderão participar de uma **família financeira**, permitindo uma visão agregada da situação financeira do grupo.

O sistema também possuirá dois indicadores de saúde financeira:

* **Saúde Financeira do Usuário**;
* **Saúde Financeira da Família**.

Esses indicadores analisarão o comportamento financeiro ao longo do tempo e apresentarão uma interpretação da situação atual.

A primeira versão será desenvolvida com foco em aprendizado e portfólio, sem integrações bancárias automáticas ou inteligência artificial.

---

# 2. Objetivo

O objetivo do Clareza é permitir que uma pessoa consiga:

* Registrar sua renda;
* Registrar suas despesas;
* Registrar seus investimentos;
* Organizar suas movimentações por categorias;
* Registrar compras à vista;
* Registrar compras no cartão;
* Registrar compras parceladas;
* Registrar despesas recorrentes;
* Acompanhar parcelas futuras;
* Visualizar sua situação financeira;
* Acompanhar sua evolução financeira;
* Visualizar gastos de meses anteriores;
* Acompanhar os gastos do mês atual;
* Visualizar compromissos financeiros já conhecidos para os próximos meses;
* Avaliar sua própria Saúde Financeira;
* Participar de uma família financeira;
* Acompanhar a situação financeira agregada da família;
* Avaliar a Saúde Financeira da Família.

Futuramente, o sistema poderá utilizar inteligência artificial para realizar previsões mais avançadas sobre os gastos futuros.

---

# 3. Público-Alvo

O sistema será direcionado principalmente para:

* Pessoas que desejam controlar suas finanças;
* Pessoas que desejam acompanhar seus investimentos manualmente;
* Pessoas que possuem compras parceladas;
* Pessoas que possuem despesas recorrentes;
* Famílias que desejam acompanhar sua situação financeira em conjunto;
* Pessoas que desejam compreender melhor seus hábitos financeiros;
* Pessoas que desejam identificar possíveis problemas financeiros antes que eles aconteçam.

---

# 4. Escopo da Primeira Versão

A primeira versão do Clareza terá:

* Cadastro de usuários;
* Login;
* Autenticação;
* Perfil do usuário;
* Criação de família;
* Código único de família;
* Solicitação para entrar em uma família;
* Aprovação ou rejeição de solicitações;
* Administrador da família;
* Chefe da família;
* Entrada e saída de membros;
* Cadastro de entradas;
* Cadastro de despesas;
* Cadastro de investimentos;
* Cadastro de compras à vista;
* Cadastro de compras no cartão;
* Cadastro de compras parceladas;
* Cadastro de despesas recorrentes;
* Controle de parcelas ativas;
* Categorias predefinidas;
* Categorias personalizadas;
* Dashboard individual;
* Dashboard familiar;
* Histórico financeiro;
* Projeção de gastos futuros baseada em compromissos conhecidos;
* Saúde Financeira do Usuário;
* Saúde Financeira da Família;
* Controle de permissões;
* API REST;
* Banco de dados MySQL.

A previsão de gastos utilizando inteligência artificial ficará fora da primeira versão.

---

# 5. Usuários

Cada pessoa deverá possuir uma conta de usuário.

Um usuário poderá pertencer a **apenas uma família por vez**.

Um usuário que não pertence a nenhuma família poderá:

* Criar uma família;
* Solicitar entrada em uma família existente através do código da família.

Um usuário que já pertence a uma família não poderá solicitar entrada em outra família enquanto permanecer na família atual.

Caso saia da família, poderá posteriormente criar ou solicitar entrada em outra.

Um usuário não precisa obrigatoriamente pertencer a uma família para utilizar o Clareza.

O usuário poderá utilizar individualmente:

* Seu controle financeiro;
* Seu dashboard;
* Sua Saúde Financeira;
* Suas projeções.

---

# 6. Famílias

Uma família representa um grupo de usuários que deseja acompanhar suas finanças de forma conjunta.

Cada família deverá possuir:

* Identificador único;
* Nome;
* Código único para entrada;
* Membros;
* Administrador(es);
* Chefe da família, quando definido.

Uma família poderá existir sem possuir um chefe.

Uma família poderá existir com apenas um membro.

A participação em uma família é opcional para o usuário.

---

# 7. Código da Família

Cada família possuirá um código único utilizado para solicitar entrada.

Exemplo:

`CLZ-8F42K`

O código deverá permitir que um usuário identifique a família à qual deseja solicitar entrada.

O código não deverá permitir entrada automática.

O código apenas iniciará o processo de solicitação.

---

# 8. Entrada em uma Família

O processo de entrada em uma família funcionará da seguinte forma:

1. O usuário informa o código da família;
2. O sistema verifica se o código existe;
3. O sistema verifica se o usuário já pertence a uma família;
4. O sistema verifica se já existe uma solicitação válida;
5. O sistema cria uma solicitação de entrada;
6. A solicitação fica com status `PENDENTE`;
7. Um administrador da família visualiza a solicitação;
8. O administrador pode aceitar ou rejeitar;
9. Caso aceite, o usuário passa a fazer parte da família;
10. Caso rejeite, o usuário não entra na família.

A entrada nunca deverá ocorrer automaticamente apenas porque o usuário possui o código.

---

# 9. Solicitações de Entrada

Uma solicitação de entrada deverá possuir informações como:

* Usuário solicitante;
* Família solicitada;
* Data da solicitação;
* Status.

Os possíveis status são:

* `PENDENTE`;
* `ACEITA`;
* `REJEITADA`.

Uma solicitação aceita adicionará o usuário à família.

Uma solicitação rejeitada não adicionará o usuário à família.

O sistema deverá impedir solicitações inválidas, como um usuário solicitar entrada em uma família enquanto já pertence a outra.

---

# 10. Papéis dentro da Família

O Clareza terá papéis diferentes dentro de uma família.

Os papéis serão:

* Criador;
* Administrador;
* Chefe;
* Membro.

Esses papéis possuem responsabilidades diferentes.

Um mesmo usuário poderá possuir mais de um papel.

Por exemplo, o criador de uma família poderá inicialmente ser:

* Criador;
* Administrador;
* Chefe.

Entretanto, esses papéis não serão obrigatoriamente permanentes ou inseparáveis.

---

# 11. Criador da Família

O criador é o usuário responsável pela criação inicial da família.

O criador:

* Cria a família;
* Recebe inicialmente o papel de administrador;
* Poderá receber o papel de chefe;
* Poderá deixar de ser administrador posteriormente;
* Poderá sair da família, desde que as regras de saída sejam respeitadas.

O papel de criador representa quem iniciou a família, mas não determina permanentemente quem possui as responsabilidades administrativas.

---

# 12. Administrador da Família

O administrador é responsável pelas ações administrativas da família.

Entre suas responsabilidades estão:

* Visualizar solicitações de entrada;
* Aceitar solicitações;
* Rejeitar solicitações;
* Gerenciar a entrada de novos membros;
* Realizar outras ações administrativas que possam ser adicionadas futuramente.

O administrador não necessariamente será o chefe da família.

Também não necessariamente será o criador.

Ser administrador não concede automaticamente acesso aos dados financeiros individuais dos membros.

A responsabilidade administrativa poderá ser transferida para outro membro autorizado.

---

# 13. Chefe da Família

O chefe da família representa um papel relacionado à visualização financeira dos membros.

Uma família poderá possuir:

* Zero chefes;
* Um chefe.

Não será permitido possuir mais de um chefe simultaneamente.

O chefe poderá visualizar os dados financeiros individuais dos membros da própria família.

O chefe nunca poderá visualizar dados financeiros de usuários que não pertencem à sua família.

Ser chefe não concede automaticamente permissões administrativas.

---

# 14. Ausência de Chefe

Uma família poderá existir sem chefe.

Quando não houver chefe:

* A família continuará funcionando normalmente;
* Os membros continuarão pertencendo à família;
* Cada membro poderá visualizar seus próprios dados financeiros;
* Todos os membros poderão visualizar a Saúde da Família;
* Nenhum membro comum poderá visualizar os dados financeiros individuais dos outros membros.

Posteriormente, um administrador poderá definir um novo chefe.

---

# 15. Saída da Família

Um usuário poderá sair da família da qual participa.

Ao sair:

* O usuário deixa de pertencer à família;
* O usuário não terá mais acesso aos dados daquela família;
* O usuário não poderá mais visualizar a Saúde da Família;
* O usuário continuará possuindo sua própria conta;
* Seus dados financeiros individuais continuarão pertencendo ao seu usuário.

Após sair, poderá criar uma nova família ou solicitar entrada em outra.

---

# 16. Saída do Administrador

Caso um administrador deixe a família, outro administrador deverá assumir suas responsabilidades.

O sistema não deverá permitir que uma família fique sem capacidade administrativa quando existirem solicitações ou ações administrativas pendentes.

A responsabilidade administrativa poderá ser transferida para outro membro autorizado.

---

# 17. Saída do Chefe

Caso o chefe deixe a família:

* O papel de chefe ficará vazio;
* A família continuará existindo;
* Os demais membros continuarão pertencendo à família;
* Os membros continuarão visualizando seus próprios dados;
* A Saúde da Família continuará disponível;
* Um novo chefe poderá ser definido posteriormente.

A saída do chefe não deverá excluir a família.

---

# 18. Privacidade dos Dados Financeiros

Os dados financeiros individuais deverão possuir controle de acesso.

Um usuário poderá visualizar:

* Seus próprios dados financeiros;
* Dados familiares agregados;
* A Saúde da Família.

Um chefe poderá visualizar:

* Seus próprios dados;
* Dados financeiros individuais dos membros da própria família;
* Dados familiares agregados;
* A Saúde da Família.

Um administrador que não seja chefe não terá acesso automático aos dados financeiros individuais dos membros.

Ser administrador é uma permissão administrativa, não uma permissão financeira.

---

# 19. Movimentações Financeiras

Uma movimentação financeira deverá possuir, conceitualmente:

* Identificador;
* Usuário responsável;
* Tipo;
* Valor;
* Descrição;
* Data;
* Categoria.

Os tipos iniciais serão:

* `ENTRADA`;
* `DESPESA`;
* `INVESTIMENTO`.

A movimentação também poderá possuir informações específicas relacionadas a:

* Forma de pagamento;
* Parcelamento;
* Recorrência;
* Datas futuras;
* Status.

Essas informações deverão ser modeladas de acordo com o tipo de movimentação.

---

# 20. Entradas

Entradas representam valores recebidos pelo usuário.

Exemplos:

* Salário;
* Freelance;
* Pagamento;
* Outros recebimentos.

Uma entrada deverá possuir pelo menos:

* Valor;
* Data;
* Descrição;
* Categoria.

Entradas poderão ser utilizadas nos cálculos de Saúde Financeira e nos dashboards.

---

# 21. Despesas

Despesas representam valores gastos ou comprometidos pelo usuário.

Exemplos:

* Alimentação;
* Moradia;
* Transporte;
* Saúde;
* Educação;
* Lazer;
* Compras;
* Assinaturas.

Uma despesa deverá possuir pelo menos:

* Valor;
* Data;
* Descrição;
* Categoria.

Uma despesa poderá ocorrer de diferentes formas, como:

* Compra à vista;
* Compra no cartão;
* Compra parcelada;
* Despesa recorrente.

---

# 22. Compras à Vista

O sistema deverá permitir o registro de compras realizadas à vista.

Uma compra à vista poderá utilizar diferentes formas de pagamento.

Exemplos:

* Dinheiro;
* PIX;
* Débito;
* Outras formas que possam ser adicionadas posteriormente.

A compra à vista deverá representar uma despesa realizada em uma única ocorrência.

---

# 23. Compras no Cartão

O sistema deverá permitir o registro de compras realizadas utilizando cartão.

Uma compra no cartão deverá estar associada a um cartão cadastrado pelo usuário.

O sistema poderá armazenar informações como:

* Cartão;
* Valor;
* Data da compra;
* Descrição;
* Categoria;
* Forma de pagamento.

O controle de cartão deverá permitir que as compras sejam utilizadas nos cálculos de gastos e compromissos futuros.

---

# 24. Compras Parceladas

O sistema deverá permitir o registro de compras parceladas.

Uma compra parcelada deverá possuir informações como:

* Valor total;
* Quantidade de parcelas;
* Valor das parcelas;
* Número da parcela;
* Data da compra;
* Datas de vencimento;
* Cartão utilizado;
* Categoria;
* Status das parcelas.

Exemplo:

```text
Compra:
Notebook

Valor:
R$ 3.000

Parcelas:
10

Valor por parcela:
R$ 300
```

Cada parcela deverá representar um compromisso financeiro específico.

Parcelas futuras deverão ser consideradas na projeção de gastos.

---

# 25. Parcelas Ativas

O sistema deverá identificar parcelas que ainda possuem valores a serem pagos.

Exemplo:

```text
Compra: Notebook
10 parcelas

Pagas:
1, 2, 3

Ativas:
4, 5, 6, 7, 8, 9, 10
```

As parcelas ainda não pagas deverão ser consideradas nos compromissos financeiros futuros.

O sistema deverá conseguir identificar quanto o usuário já possui comprometido para os próximos meses.

---

# 26. Despesas Recorrentes

O sistema deverá permitir o cadastro de despesas recorrentes.

Exemplos:

* Aluguel;
* Internet;
* Academia;
* Streaming;
* Plano de celular;
* Seguro;
* Assinaturas.

Uma despesa recorrente poderá possuir:

* Descrição;
* Valor;
* Categoria;
* Periodicidade;
* Data inicial;
* Data final, quando aplicável;
* Forma de pagamento;
* Status.

A periodicidade poderá ser inicialmente mensal.

Outras periodicidades poderão ser adicionadas posteriormente.

---

# 27. Diferença entre Parcelamento e Recorrência

O sistema deverá tratar parcelamentos e recorrências como conceitos diferentes.

### Parcelamento

Possui uma quantidade definida de parcelas.

Exemplo:

```text
Celular
12 parcelas
```

Após a última parcela, o compromisso termina.

### Recorrência

Continua acontecendo enquanto estiver ativa.

Exemplo:

```text
Aluguel
Mensal
```

A recorrência poderá continuar até ser encerrada.

Essa diferença deverá ser considerada na modelagem do banco e nos cálculos de projeção.

---

# 28. Cartões

O sistema poderá permitir o cadastro de cartões utilizados pelo usuário.

Um cartão poderá possuir informações como:

* Identificador;
* Nome;
* Limite;
* Dia de fechamento;
* Dia de vencimento;
* Status.

As compras realizadas no cartão deverão estar associadas ao cartão correspondente.

O controle de cartões será utilizado principalmente para acompanhar compras e parcelas.

Funcionalidades avançadas de fatura poderão ser adicionadas posteriormente.

---

# 29. Investimentos

Investimentos representam valores direcionados para investimentos ou reservas financeiras.

Investimento não será tratado como uma despesa comum.

Também não será tratado como uma entrada.

A primeira versão permitirá apenas o registro manual dos investimentos.

O sistema não realizará inicialmente:

* Integração com corretoras;
* Consulta automática de rentabilidade;
* Compra ou venda de ativos;
* Atualização automática de valores.

O investimento será considerado nas análises financeiras do usuário e da família.

---

# 30. Categorias

O sistema possuirá categorias predefinidas.

Exemplos:

* Alimentação;
* Moradia;
* Transporte;
* Saúde;
* Educação;
* Lazer;
* Salário;
* Investimentos;
* Outros.

Também será permitido ao usuário criar categorias personalizadas.

Exemplos:

* Pets;
* Academia;
* Jogos;
* Meu negócio.

As categorias personalizadas deverão ser associadas ao usuário ou a uma estrutura definida pelo sistema para evitar conflitos entre usuários.

---

# 31. Dashboard Individual

Cada usuário terá acesso a um dashboard individual.

O dashboard poderá apresentar:

* Saldo atual;
* Total de entradas;
* Total de despesas;
* Total de investimentos;
* Gastos por categoria;
* Evolução mensal;
* Comparação entre entradas e despesas;
* Capacidade de poupança/investimento;
* Compras parceladas;
* Despesas recorrentes;
* Compromissos financeiros futuros.

Os dados deverão ser apresentados por meio de:

* Cards;
* Tabelas;
* Gráficos;
* Indicadores.

---

# 32. Dashboard Familiar

A família possuirá uma visão financeira agregada.

O dashboard familiar poderá apresentar:

* Total de entradas da família;
* Total de despesas;
* Total de investimentos;
* Evolução financeira;
* Gastos por categoria;
* Comparação entre entradas e despesas;
* Capacidade de poupança/investimento da família;
* Compromissos financeiros futuros agregados.

Membros comuns não deverão visualizar os valores individuais dos outros membros.

O chefe poderá visualizar informações financeiras individuais dos membros da própria família.

---

# 33. Histórico Financeiro

O Clareza deverá permitir a visualização do comportamento financeiro ao longo do tempo.

Os gráficos poderão apresentar informações referentes a:

* Meses anteriores;
* Mês atual;
* Próximo mês;
* Outros períodos futuros quando aplicável.

Os meses anteriores deverão utilizar os dados financeiros efetivamente registrados.

O histórico poderá apresentar:

* Total de entradas;
* Total de despesas;
* Total de investimentos;
* Gastos por categoria;
* Evolução do saldo;
* Evolução da capacidade de poupança.

---

# 34. Projeção de Gastos

O Clareza deverá permitir uma projeção de gastos futuros baseada em compromissos financeiros já conhecidos pelo sistema.

Na primeira versão, essa projeção será determinística.

Isso significa que o sistema não tentará adivinhar gastos que ainda não foram registrados.

A projeção deverá considerar principalmente:

* Parcelas ativas;
* Despesas recorrentes ativas;
* Outros compromissos financeiros já registrados.

Exemplo:

```text
Próximo mês

Aluguel              R$ 1.500
Internet              R$ 120
Academia              R$ 100
Parcela Notebook      R$ 300
Parcela Celular       R$ 150
──────────────────────────────
Comprometido        R$ 2.170
```

O sistema deverá apresentar esse valor como:

**Gastos já confirmados ou comprometidos.**

Esse valor não deverá ser apresentado como uma previsão exata do gasto total do usuário.

---

# 35. Diferença entre Gasto Confirmado e Previsão

O Clareza deverá diferenciar dois conceitos.

### Gasto confirmado/projetado

Valor que o sistema consegue determinar com base em informações já registradas.

Exemplos:

* Parcela ativa;
* Aluguel recorrente;
* Assinatura recorrente.

### Previsão de gasto

Estimativa de quanto o usuário provavelmente gastará, considerando também comportamentos que ainda não foram registrados.

Essa segunda modalidade será implementada futuramente com recursos de análise avançada e inteligência artificial.

---

# 36. Previsão de Gastos com Inteligência Artificial

Futuramente, o Clareza poderá utilizar inteligência artificial para estimar o gasto total futuro do usuário ou da família.

A previsão poderá considerar:

* Histórico financeiro;
* Comportamento de consumo;
* Gastos recorrentes;
* Parcelamentos;
* Categorias;
* Evolução dos gastos;
* Sazonalidade;
* Outros padrões identificados nos dados.

Exemplo:

```text
Próximo mês

Comprometido:
R$ 2.170

Previsão de gasto total:
R$ 3.450
```

Nesse exemplo:

**R$ 2.170** representa valores já conhecidos.

**R$ 3.450** representa uma estimativa.

A previsão por inteligência artificial não deverá substituir os dados financeiros reais.

---

# 37. Saúde Financeira do Usuário

Todo usuário deverá possuir uma análise própria de Saúde Financeira.

Essa funcionalidade deverá funcionar mesmo que o usuário não pertença a nenhuma família.

A Saúde Financeira do Usuário deverá analisar o comportamento financeiro individual.

Entre os fatores que poderão ser considerados estão:

* Entradas;
* Despesas;
* Investimentos;
* Capacidade de poupança;
* Evolução das despesas;
* Relação entre renda e gastos;
* Gastos recorrentes;
* Parcelamentos;
* Compromissos futuros;
* Evolução ao longo dos meses.

---

# 38. Estados da Saúde Financeira

A Saúde Financeira poderá utilizar estados interpretativos e dinâmicos.

Exemplos:

* Saúde de ferro;
* Saudável;
* Se recuperando;
* Sentindo uma dor de cabeça;
* Sentindo sintomas ruins;
* Enxaqueca pesada.

Os nomes poderão ser ajustados durante o desenvolvimento.

O objetivo é evitar uma classificação excessivamente simples baseada apenas em cores ou em um único valor financeiro.

---

# 39. Análise da Saúde do Usuário

A Saúde Financeira deverá considerar tendências.

Exemplo:

```text
Agosto
Despesas: R$ 2.500

Setembro
Despesas: R$ 3.000

Outubro
Despesas: R$ 3.800
```

Caso a renda permaneça estável e a capacidade de poupança diminua, o sistema poderá identificar uma tendência negativa.

A análise deverá considerar mais do que apenas o saldo atual.

---

# 40. Explicação da Saúde do Usuário

A Saúde Financeira deverá, sempre que possível, apresentar os motivos que contribuíram para o estado atual.

Exemplo:

```text
Saúde Financeira:
Sentindo uma dor de cabeça

Motivos:

- Despesas aumentaram nos últimos 3 meses;
- Capacidade de poupança diminuiu;
- Gastos recorrentes representam uma parcela significativa da renda;
- Existem parcelas ativas para os próximos meses.
```

Isso permitirá que o usuário compreenda sua situação financeira.

---

# 41. Saúde Financeira da Família

O Clareza também possuirá uma Saúde Financeira da Família.

Essa análise deverá considerar os dados financeiros agregados da família.

Entre os fatores que poderão ser considerados:

* Entradas da família;
* Despesas da família;
* Investimentos;
* Capacidade de poupança;
* Evolução das despesas;
* Evolução das entradas;
* Gastos recorrentes;
* Parcelamentos;
* Compromissos futuros;
* Tendências financeiras.

---

# 42. Privacidade da Saúde da Família

Todos os membros da família poderão visualizar a Saúde da Família.

Entretanto, a Saúde da Família não deverá revelar dados financeiros individuais dos membros para usuários que não possuem essa permissão.

Por exemplo:

```text
Saúde da Família:
Se recuperando

Motivos:
- Despesas familiares diminuíram;
- Capacidade de poupança aumentou;
- Investimentos aumentaram.
```

Um membro comum não deverá receber automaticamente:

```text
João gastou R$ 2.300
Maria gastou R$ 1.800
Felipe gastou R$ 900
```

A menos que possua permissão para visualizar esses dados.

---

# 43. Histórico da Saúde

Futuramente, o sistema poderá armazenar o histórico das análises de Saúde Financeira.

Isso poderá permitir visualizar a evolução do usuário ou da família.

Exemplo:

```text
Janeiro:
Saudável

Fevereiro:
Saudável

Março:
Sentindo uma dor de cabeça

Abril:
Se recuperando

Maio:
Saúde de ferro
```

Essa funcionalidade poderá ser implementada posteriormente.

---

# 44. API REST

O backend deverá disponibilizar uma API REST para comunicação com o frontend.

O frontend React não deverá acessar diretamente o banco de dados.

A comunicação deverá seguir:

```text
React + TypeScript
        ↓
HTTP / JSON
        ↓
Node.js + Express + TypeScript
        ↓
SQL
        ↓
MySQL
```

A API será responsável por:

* Autenticação;
* Autorização;
* Validação;
* Regras de negócio;
* Manipulação das movimentações;
* Controle das famílias;
* Controle das permissões;
* Controle das compras;
* Controle dos parcelamentos;
* Controle das recorrências;
* Cálculos financeiros;
* Comunicação com o banco de dados.

---

# 45. Autenticação

O sistema deverá possuir autenticação de usuários.

O usuário deverá realizar login para acessar seus dados.

As senhas:

* Não deverão ser armazenadas em texto puro;
* Deverão utilizar hash seguro;
* Não deverão ser retornadas pela API.

---

# 46. Autorização

O sistema deverá diferenciar autenticação de autorização.

A autenticação verificará:

> "Quem é esse usuário?"

A autorização verificará:

> "Esse usuário pode acessar esse recurso?"

Exemplos:

* Um usuário pode acessar suas próprias movimentações;
* Um chefe pode acessar movimentações individuais dos membros da própria família;
* Um administrador pode gerenciar solicitações;
* Um usuário não pode acessar dados de outra família.

---

# 47. Requisitos Funcionais

### RF01 — Cadastro de usuário

O sistema deverá permitir o cadastro de novos usuários.

### RF02 — Login

O sistema deverá permitir que usuários realizem login.

### RF03 — Perfil

O sistema deverá permitir que o usuário visualize e altere informações permitidas do seu perfil.

### RF04 — Criar família

O sistema deverá permitir que um usuário sem família crie uma família.

### RF05 — Código da família

O sistema deverá gerar um código único para cada família.

### RF06 — Solicitar entrada

O sistema deverá permitir que um usuário sem família solicite entrada em uma família utilizando o código.

### RF07 — Solicitação pendente

O sistema deverá registrar uma solicitação com status `PENDENTE`.

### RF08 — Visualizar solicitações

O sistema deverá permitir que administradores visualizem solicitações de entrada da própria família.

### RF09 — Aceitar solicitação

O sistema deverá permitir que um administrador aceite uma solicitação.

### RF10 — Rejeitar solicitação

O sistema deverá permitir que um administrador rejeite uma solicitação.

### RF11 — Entrada na família

Após aprovação, o usuário deverá passar a fazer parte da família.

### RF12 — Restrição de família

O sistema deverá impedir que um usuário pertença a mais de uma família simultaneamente.

### RF13 — Sair da família

O sistema deverá permitir que um usuário saia da família.

### RF14 — Gerenciar administrador

O sistema deverá permitir a transferência do papel de administrador para outro membro autorizado.

### RF15 — Definir chefe

O sistema deverá permitir que um administrador defina um membro como chefe da família.

### RF16 — Remover chefe

O sistema deverá permitir que o papel de chefe seja removido.

### RF17 — Registrar entrada

O sistema deverá permitir o registro de entradas financeiras.

### RF18 — Registrar despesa

O sistema deverá permitir o registro de despesas.

### RF19 — Registrar investimento

O sistema deverá permitir o registro de investimentos.

### RF20 — Registrar compra à vista

O sistema deverá permitir o registro de compras realizadas à vista.

### RF21 — Registrar compra no cartão

O sistema deverá permitir o registro de compras realizadas utilizando cartão.

### RF22 — Registrar compra parcelada

O sistema deverá permitir o registro de compras parceladas.

### RF23 — Registrar despesa recorrente

O sistema deverá permitir o cadastro de despesas recorrentes.

### RF24 — Controlar parcelas

O sistema deverá identificar parcelas pagas e parcelas ainda ativas.

### RF25 — Listar movimentações

O sistema deverá permitir a visualização das movimentações do usuário.

### RF26 — Editar movimentação

O sistema deverá permitir a edição de movimentações do usuário.

### RF27 — Excluir movimentação

O sistema deverá permitir a exclusão de movimentações do usuário.

### RF28 — Categorias

O sistema deverá disponibilizar categorias predefinidas.

### RF29 — Categorias personalizadas

O sistema deverá permitir a criação de categorias personalizadas.

### RF30 — Dashboard individual

O sistema deverá apresentar informações financeiras individuais do usuário.

### RF31 — Dashboard familiar

O sistema deverá apresentar informações financeiras agregadas da família.

### RF32 — Histórico financeiro

O sistema deverá permitir a visualização da evolução financeira em diferentes períodos.

### RF33 — Projeção financeira

O sistema deverá apresentar uma projeção dos gastos futuros com base em parcelas ativas, despesas recorrentes e outros compromissos financeiros conhecidos.

### RF34 — Saúde Financeira do Usuário

O sistema deverá calcular e apresentar a Saúde Financeira individual do usuário.

### RF35 — Saúde Financeira da Família

O sistema deverá calcular e apresentar a Saúde Financeira da família.

### RF36 — Análise financeira

O sistema deverá analisar indicadores financeiros ao longo do tempo.

### RF37 — Controle de acesso

O sistema deverá impedir o acesso a recursos não autorizados.

### RF38 — Dados financeiros do chefe

O sistema deverá permitir que o chefe visualize os dados financeiros individuais dos membros da própria família.

### RF39 — Previsão inteligente

Futuramente, o sistema poderá utilizar inteligência artificial para estimar o gasto total futuro com base no histórico e comportamento financeiro.

---

# 48. Requisitos Não Funcionais

### RNF01 — Segurança

O sistema deverá proteger os dados dos usuários e utilizar hash seguro para senhas.

### RNF02 — Autenticação

Recursos privados deverão exigir autenticação.

### RNF03 — Autorização

O backend deverá validar as permissões do usuário antes de disponibilizar informações protegidas.

### RNF04 — Banco de dados

O sistema deverá utilizar MySQL.

### RNF05 — API

O backend deverá disponibilizar uma API REST.

### RNF06 — Separação de responsabilidades

O frontend não deverá acessar diretamente o banco de dados.

### RNF07 — Responsividade

A interface deverá funcionar adequadamente em diferentes tamanhos de tela.

### RNF08 — TypeScript

Frontend e backend deverão utilizar TypeScript.

### RNF09 — Manutenibilidade

O código deverá ser organizado de forma a facilitar manutenção e evolução.

### RNF10 — Integridade

O sistema deverá impedir inconsistências relacionadas à participação dos usuários nas famílias, movimentações financeiras e permissões de acesso.

### RNF11 — Consistência financeira

Os cálculos financeiros deverão considerar corretamente movimentações, parcelas, recorrências e compromissos futuros.

### RNF12 — Privacidade

Informações financeiras individuais deverão ser disponibilizadas somente para usuários autorizados.

---

# 49. Fora do Escopo da Primeira Versão

Os seguintes recursos não farão parte da primeira versão:

* Open Banking;
* Integração automática com bancos;
* Importação automática de extratos;
* Inteligência Artificial;
* Previsão inteligente de gastos;
* Integração com corretoras;
* Rentabilidade automática de investimentos;
* Compra ou venda automática de ativos;
* PIX;
* Pagamentos;
* Cartão de crédito integrado ao sistema bancário;
* Aplicativo mobile nativo;
* Análises financeiras avançadas baseadas em IA.

---

# 50. Possíveis Evoluções

Após a conclusão da primeira versão, poderão ser adicionados:

* Open Banking;
* Integração com bancos;
* Inteligência Artificial;
* Previsão de gastos;
* Análise financeira avançada;
* Recomendações personalizadas;
* Alertas financeiros;
* Metas financeiras;
* Orçamentos;
* Investimentos integrados;
* Aplicativo mobile;
* Histórico avançado da Saúde Financeira;
* Mais níveis de permissão;
* Notificações;
* Controle avançado de faturas;
* Integração com cartões;
* Mais tipos de recorrência.

---

# 51. Princípio de Desenvolvimento

O Clareza será desenvolvido inicialmente como um projeto de estudo e portfólio.

A prioridade será construir uma aplicação funcional utilizando conceitos reais de desenvolvimento de software, incluindo:

* Frontend;
* Backend;
* API REST;
* Banco de dados;
* Autenticação;
* Autorização;
* Regras de negócio;
* Controle de acesso;
* Modelagem de dados;
* Versionamento com Git.

A implementação deverá evitar complexidade desnecessária, mas manter uma arquitetura que permita evolução futura.

O projeto deverá priorizar primeiro soluções determinísticas e baseadas em dados concretos.

Recursos de inteligência artificial deverão ser adicionados somente posteriormente, quando houver dados e estrutura suficientes para justificar sua utilização.

---

# 52. Tecnologias

## Frontend

* React;
* TypeScript;
* HTML;
* CSS.

## Backend

* Node.js;
* TypeScript;
* Express.

## Banco de dados

* MySQL.

## Versionamento

* Git;
* GitHub.

---

# 53. Arquitetura Inicial

```text
                    ┌─────────────────────┐
                    │      FRONTEND       │
                    │   React + TypeScript│
                    └──────────┬──────────┘
                               │
                         HTTP / JSON
                               │
                               ▼
                    ┌─────────────────────┐
                    │       BACKEND       │
                    │ Node + Express + TS │
                    └──────────┬──────────┘
                               │
                              SQL
                               │
                               ▼
                    ┌─────────────────────┐
                    │      DATABASE       │
                    │        MySQL        │
                    └─────────────────────┘
```

---

# 54. Conceito Geral da Análise Financeira

O Clareza deverá separar os dados financeiros reais das análises derivadas desses dados.

O fluxo conceitual será:

```text
Dados financeiros
       ↓
Movimentações
       ↓
Parcelas e recorrências
       ↓
Indicadores financeiros
       ↓
Análise
       ↓
Saúde Financeira
       ↓
Projeções
```

A primeira versão deverá utilizar regras e cálculos determinísticos.

Futuramente, recursos de inteligência artificial poderão utilizar os mesmos dados para gerar previsões e análises mais avançadas.

---

# 55. Status do Projeto

O projeto encontra-se na fase de **levantamento e definição de requisitos**.

Próximas etapas:

1. Revisar requisitos;
2. Definir regras de negócio;
3. Identificar entidades;
4. Definir atributos;
5. Definir relacionamentos;
6. Criar modelo entidade-relacionamento;
7. Criar banco de dados MySQL;
8. Criar backend;
9. Criar API REST;
10. Implementar autenticação;
11. Implementar autorização;
12. Implementar movimentações;
13. Implementar compras e parcelamentos;
14. Implementar despesas recorrentes;
15. Implementar dashboards;
16. Implementar projeções financeiras;
17. Implementar Saúde Financeira do Usuário;
18. Implementar Saúde Financeira da Família;
19. Testar e documentar o sistema;
20. Posteriormente estudar a implementação de previsão com inteligência artificial.
