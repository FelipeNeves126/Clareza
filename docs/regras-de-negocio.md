# Regras de Negócio — Clareza

## 1. Objetivo

Este documento define as regras que determinam como as informações e operações do Clareza devem funcionar.

As regras de negócio complementam os requisitos funcionais e servem como referência para a modelagem do banco de dados, desenvolvimento da API, implementação do frontend e definição das regras de autorização.

---

# 2. Usuários

### RN01 — Cadastro de usuário

Cada usuário deve possuir uma conta própria no sistema.

Um usuário não pode possuir mais de uma conta utilizando o mesmo e-mail.

### RN02 — Usuário sem família

Um usuário pode utilizar o Clareza sem pertencer a uma família.

Nesse caso, ele pode utilizar as funcionalidades individuais disponíveis para sua conta.

### RN03 — Uma família por usuário

Um usuário pode pertencer a, no máximo, uma família simultaneamente.

O sistema não deve permitir que o mesmo usuário esteja vinculado a duas famílias ao mesmo tempo.

### RN04 — Saída da família

Um usuário pode sair da família da qual participa.

Após sair, ele deixa de possuir acesso às informações e funcionalidades exclusivas daquela família.

O usuário poderá posteriormente criar ou solicitar entrada em outra família.

---

# 3. Famílias

### RN05 — Criação de família

Um usuário pode criar uma família.

O usuário que criar a família será inicialmente responsável pela administração dela.

### RN06 — Código único da família

Cada família deve possuir um código único utilizado para identificar a família durante o processo de entrada de novos membros.

Duas famílias não podem possuir o mesmo código.

### RN07 — Código não concede acesso automaticamente

Possuir o código de uma família não adiciona automaticamente o usuário à família.

O usuário deverá solicitar a entrada utilizando o código.

### RN08 — Solicitação de entrada

A entrada em uma família deve ocorrer por meio de uma solicitação.

A solicitação deverá permanecer pendente até que um administrador da respectiva família tome uma decisão.

### RN09 — Aprovação da solicitação

Somente um usuário com função de administrador daquela família poderá aprovar uma solicitação de entrada.

Após a aprovação, o usuário passa a fazer parte da família.

### RN10 — Rejeição da solicitação

Um administrador poderá rejeitar uma solicitação de entrada.

Uma solicitação rejeitada não deve adicionar o usuário à família.

### RN11 — Solicitação pendente não concede acesso

Enquanto uma solicitação estiver pendente, o usuário não será considerado membro da família e não terá acesso às informações exclusivas da família.

### RN12 — Solicitações duplicadas

O sistema não deve permitir múltiplas solicitações de entrada pendentes do mesmo usuário para a mesma família.

---

# 4. Funções e permissões dentro da família

O Clareza possui quatro funções que podem ser atribuídas aos membros:

* Criador
* Administrador
* Chefe
* Membro

Essas funções representam responsabilidades e permissões diferentes.

### RN13 — Função de criador

O usuário responsável pela criação da família recebe inicialmente a função de criador.

A função de criador não deve ser confundida automaticamente com as funções de administrador ou chefe.

### RN14 — Função de administrador

Um administrador é responsável, entre outras tarefas, por gerenciar as solicitações de entrada na família.

Ser administrador não concede automaticamente acesso aos dados financeiros individuais dos demais membros.

### RN15 — Função de chefe

O chefe possui permissão para visualizar informações financeiras individuais dos membros da própria família, conforme as regras de privacidade do sistema.

A função de chefe é independente da função de administrador.

### RN16 — Acúmulo de funções

Um usuário pode possuir mais de uma função dentro da mesma família.

Por exemplo, um usuário pode ser simultaneamente administrador e chefe.

### RN17 — No máximo um chefe

Uma família pode possuir no máximo um chefe simultaneamente.

Uma família também pode existir sem nenhum chefe.

### RN18 — Chefe não é automaticamente administrador

Receber a função de chefe não concede automaticamente a função de administrador.

As permissões devem ser tratadas separadamente.

### RN19 — Administrador não possui acesso financeiro automático

Receber a função de administrador não concede automaticamente acesso aos dados financeiros individuais dos demais membros.

As permissões administrativas e financeiras são independentes.

### RN20 — Visualização dos próprios dados

Todo usuário autenticado poderá visualizar seus próprios dados financeiros, respeitando as regras de segurança e autorização do sistema.

### RN21 — Acesso do chefe aos dados dos membros

O chefe poderá visualizar informações financeiras individuais dos membros pertencentes à própria família.

O chefe não poderá visualizar informações de usuários que não pertençam à sua família.

### RN22 — Privacidade dos membros comuns

Um membro comum não poderá visualizar os dados financeiros individuais dos demais membros da família.

Ele poderá visualizar as informações agregadas da família disponibilizadas pelo sistema.

### RN23 — Informações agregadas da família

As informações financeiras apresentadas para membros que não possuem permissão para visualizar dados individuais devem ser agregadas.

Essas informações não devem permitir identificar diretamente os valores financeiros de um membro específico.

---

# 5. Administração da família

### RN24 — Saída do chefe

O chefe poderá sair da família.

Nesse caso, a família continuará existindo e poderá permanecer temporariamente sem chefe.

### RN25 — Saída de administrador

A saída de um administrador não deve impedir que a família continue sendo administrada.

Quando necessário, outro administrador deverá assumir a responsabilidade pelas operações administrativas.

### RN26 — Família sem administrador

O sistema deve impedir que uma família fique permanentemente sem possibilidade de administração quando houver operações que dependam de um administrador.

A forma de substituição do administrador deverá ser definida pela implementação das permissões administrativas.

---

# 6. Movimentações financeiras

### RN27 — Tipos de movimentação

Uma movimentação financeira deve possuir um dos seguintes tipos:

* ENTRADA
* DESPESA
* INVESTIMENTO

### RN28 — Entrada

Uma entrada representa um valor recebido pelo usuário.

Exemplos:

* salário;
* pagamento recebido;
* renda extra;
* outros valores recebidos.

### RN29 — Despesa

Uma despesa representa um valor relacionado ao consumo, compra ou obrigação financeira do usuário.

Exemplos:

* alimentação;
* aluguel;
* transporte;
* contas;
* compras;
* parcelas.

### RN30 — Investimento

Um investimento representa um valor destinado a uma aplicação ou investimento financeiro.

Um investimento não deve ser classificado como despesa.

Um investimento também não deve ser considerado uma entrada.

### RN31 — Movimentação pertence ao usuário

Toda movimentação financeira deve estar associada ao usuário responsável por ela.

Um usuário não poderá alterar ou excluir movimentações pertencentes a outro usuário sem possuir uma permissão específica para isso.

### RN32 — Categoria da movimentação

Uma movimentação poderá possuir uma categoria.

A categoria será utilizada para organizar e analisar os gastos, entradas e investimentos.

### RN33 — Categorias predefinidas

O sistema poderá disponibilizar categorias predefinidas para facilitar o registro das movimentações.

### RN34 — Categorias personalizadas

O usuário poderá criar categorias personalizadas para organizar suas próprias movimentações.

Uma categoria personalizada criada por um usuário não deve alterar automaticamente as categorias de outros usuários.

---

# 7. Compras e formas de pagamento

### RN35 — Forma de pagamento

Uma despesa poderá ser realizada por diferentes formas de pagamento, como:

* dinheiro;
* débito;
* crédito;
* outras formas definidas pelo sistema.

A forma de pagamento não deve ser confundida com o tipo da movimentação.

Por exemplo:

> Compra no cartão de crédito = DESPESA + forma de pagamento CRÉDITO.

### RN36 — Compra à vista

Uma compra à vista representa uma despesa cujo pagamento ocorre de uma única vez.

O valor total da compra deve ser considerado no registro financeiro correspondente.

### RN37 — Compra parcelada

Uma compra parcelada representa uma despesa dividida em um número definido de parcelas.

Exemplo:

> Compra de R$ 1.000 em 10 parcelas de R$ 100.

A compra deve manter a informação de que possui 10 parcelas e cada parcela deve possuir seu próprio valor e referência temporal.

### RN38 — Parcelamento possui quantidade definida

Uma compra parcelada possui uma quantidade determinada de parcelas.

O parcelamento termina quando todas as parcelas previstas forem registradas como concluídas.

### RN39 — Parcela não é uma nova compra

As parcelas pertencem à mesma compra original.

O sistema não deve tratar cada parcela como uma compra independente.

Isso permite identificar a compra original e acompanhar suas parcelas.

---

# 8. Despesas recorrentes

### RN40 — Despesa recorrente

Uma despesa recorrente representa uma obrigação que se repete ao longo do tempo.

Exemplos:

* aluguel;
* assinatura de streaming;
* internet;
* mensalidade.

### RN41 — Recorrência é diferente de parcelamento

Uma despesa recorrente não deve ser confundida com uma compra parcelada.

Parcelamento possui uma quantidade definida de parcelas.

Recorrência continua sendo gerada enquanto estiver ativa, até que seja encerrada.

### RN42 — Encerramento da recorrência

O usuário poderá encerrar uma despesa recorrente.

Após o encerramento, novas ocorrências não deverão ser geradas.

Ocorrências anteriores permanecem registradas no histórico financeiro.

### RN43 — Histórico de despesas recorrentes

Encerrar uma despesa recorrente não deve apagar as despesas que já foram registradas anteriormente.

O histórico financeiro deve ser preservado.

---

# 9. Compromissos financeiros futuros

### RN44 — Compromisso financeiro

O sistema poderá considerar como compromisso financeiro futuro valores que já são conhecidos pelo sistema.

Exemplos:

* parcelas ainda não vencidas;
* despesas recorrentes ativas;
* outros valores futuros previamente registrados.

### RN45 — Projeção confirmada

A projeção financeira baseada em compromissos conhecidos deve representar valores que já estão registrados ou comprometidos no sistema.

Essa projeção não deve ser apresentada como uma previsão baseada em inteligência artificial.

### RN46 — Diferença entre compromisso e previsão

O sistema deve diferenciar:

**Projeção confirmada:**
valor futuro baseado em informações já registradas e conhecidas.

**Previsão inteligente:**
estimativa de valores futuros baseada em histórico, comportamento e outros fatores.

---

# 10. Projeção financeira

### RN47 — Histórico financeiro

Para períodos passados, os gráficos financeiros devem utilizar os valores efetivamente registrados no sistema.

### RN48 — Mês atual

Para o mês atual, o sistema poderá apresentar os valores já registrados juntamente com os compromissos futuros conhecidos daquele período.

A interface deve deixar clara a diferença entre valores já realizados e valores ainda previstos.

### RN49 — Meses futuros

Para períodos futuros, a projeção inicial deverá utilizar compromissos conhecidos, como:

* parcelas futuras;
* despesas recorrentes ativas;
* outros compromissos financeiros registrados.

### RN50 — Projeção não confirmada

O sistema não deve apresentar uma estimativa baseada em comportamento histórico como se fosse um valor confirmado.

Valores estimados devem ser identificados como previsão.

---

# 11. Saúde financeira individual

### RN51 — Saúde financeira do usuário

O Clareza deve calcular uma classificação de saúde financeira para cada usuário.

A classificação deve ser derivada dos dados financeiros registrados no sistema.

Ela não deve ser apenas um valor cadastrado manualmente.

### RN52 — Fatores utilizados na saúde financeira

O cálculo da saúde financeira poderá considerar fatores como:

* entradas;
* despesas;
* investimentos;
* capacidade de economia;
* despesas recorrentes;
* parcelas;
* compromissos futuros;
* distribuição dos gastos por categoria;
* evolução financeira ao longo do tempo.

### RN53 — Estados da saúde financeira

A saúde financeira poderá ser apresentada por estados expressivos, como:

* Saúde de ferro
* Saudável
* Se recuperando
* Sentindo uma dor de cabeça
* Sentindo sintomas ruins
* Enxaqueca pesada

### RN54 — Explicação da saúde financeira

O sistema deve apresentar os principais fatores que contribuíram para a classificação da saúde financeira.

A classificação não deve ser apresentada como um resultado sem explicação.

### RN55 — Período da análise

A saúde financeira deve considerar um período de análise definido pelo sistema.

O período utilizado deve ser informado ao usuário quando necessário para que o resultado possa ser interpretado corretamente.

---

# 12. Saúde financeira da família

### RN56 — Saúde financeira da família

O Clareza deve calcular uma classificação de saúde financeira para a família.

Essa classificação deve utilizar informações financeiras agregadas dos membros da família.

### RN57 — Privacidade na saúde da família

A saúde financeira da família não deve revelar automaticamente os dados financeiros individuais dos membros que não podem ser visualizados pelo usuário.

### RN58 — Acesso à saúde financeira da família

Os membros da família poderão visualizar a classificação financeira agregada da própria família, conforme as permissões definidas pelo sistema.

### RN59 — Saúde individual e saúde familiar são diferentes

A saúde financeira individual de um usuário e a saúde financeira da família são indicadores distintos.

Um usuário pode possuir uma boa saúde financeira individual enquanto a família possui uma classificação diferente, e vice-versa.

---

# 13. Previsão inteligente

### RN60 — Previsão inteligente como funcionalidade futura

A previsão inteligente baseada em inteligência artificial não faz parte da primeira versão do sistema.

Sua implementação deverá ocorrer posteriormente.

### RN61 — Dados utilizados na previsão

Quando implementada, a previsão inteligente poderá utilizar informações como:

* histórico de gastos;
* categorias;
* recorrências;
* parcelas;
* comportamento de consumo;
* períodos anteriores;
* sazonalidade;
* outros padrões identificados nos dados.

### RN62 — Previsão não é valor confirmado

Uma previsão gerada por inteligência artificial deve ser tratada como estimativa.

Ela não deve substituir os valores financeiros efetivamente registrados pelo usuário.

---

# 14. Segurança e autorização

### RN63 — Autenticação

O sistema deve identificar o usuário autenticado antes de permitir acesso às informações privadas da conta.

### RN64 — Autorização

Estar autenticado não significa possuir acesso a todas as informações.

O acesso deverá respeitar as permissões do usuário dentro da família e as regras de privacidade.

### RN65 — Isolamento dos dados financeiros

Um usuário não deve conseguir acessar diretamente os dados financeiros de outro usuário apenas alterando identificadores enviados à API.

O backend deve validar a autorização antes de retornar ou alterar informações financeiras.

### RN66 — Acesso limitado à própria família

Permissões relacionadas à família devem ser válidas somente dentro da família à qual o usuário pertence.

Um usuário não poderá utilizar uma permissão de uma família para acessar informações de outra.

---

# 15. Integridade dos dados financeiros

### RN67 — Valores financeiros

Valores financeiros devem ser armazenados e calculados com precisão adequada para valores monetários.

O sistema não deve utilizar tipos inadequados para representar dinheiro.

### RN68 — Consistência dos cálculos

Os valores apresentados nos dashboards, gráficos, projeções e indicadores devem ser derivados dos registros financeiros existentes no sistema.

O sistema não deve produzir totais incompatíveis com os dados armazenados.

### RN69 — Preservação do histórico

Alterações em configurações futuras, como encerramento de recorrências, não devem apagar o histórico financeiro já registrado.

### RN70 — Exclusão de informações financeiras

A exclusão de uma informação financeira deve respeitar as regras de integridade do sistema.

Informações relacionadas a parcelas, compras, recorrências ou outros registros dependentes não devem permanecer inconsistentes após uma alteração ou exclusão.

---

# 16. Regras que não fazem parte da primeira versão

As seguintes regras e funcionalidades não serão implementadas inicialmente:

* integração com Open Banking;
* importação automática de extratos;
* integração direta com bancos;
* previsão financeira por inteligência artificial;
* integração com corretoras;
* cálculo automático de rentabilidade de investimentos;
* compra e venda automática de ativos;
* pagamentos;
* PIX;
* integração de cartão de crédito com instituições financeiras;
* aplicativo mobile nativo;
* recursos avançados de inteligência artificial.

Esses recursos poderão ser incorporados futuramente sem alterar o princípio de que as regras de negócio devem permanecer separadas da interface do sistema.

---

# 17. Observações para a implementação

As regras deste documento devem orientar as próximas etapas do projeto.

A ordem recomendada é:

1. Revisar e validar as regras de negócio.
2. Identificar as entidades do domínio.
3. Definir os atributos de cada entidade.
4. Definir os relacionamentos.
5. Definir cardinalidades e restrições.
6. Criar o modelo entidade-relacionamento.
7. Revisar o modelo com base nas regras de negócio.
8. Criar o banco de dados.
9. Implementar o backend.
10. Implementar a API REST.
11. Implementar autenticação e autorização.
12. Implementar o frontend.
13. Implementar dashboards e projeções.
14. Implementar a saúde financeira.
15. Adicionar funcionalidades futuras, como previsão inteligente.

As regras de negócio devem ser revisadas sempre que uma nova decisão do projeto alterar o comportamento esperado do sistema.

# 17. Segurança, autenticação e proteção de dados

### RN71 — Autenticação obrigatória

O sistema deve autenticar o usuário antes de permitir acesso às funcionalidades e informações privadas de sua conta.

### RN72 — Identidade do usuário

Toda operação realizada sobre dados privados deve estar associada ao usuário autenticado.

O backend não deve confiar exclusivamente em identificadores enviados pelo cliente para determinar a identidade do usuário.

### RN73 — Autorização no backend

Toda operação que envolva dados privados deve ser autorizada pelo backend.

A interface do frontend não deve ser considerada mecanismo de segurança.

### RN74 — Isolamento dos dados dos usuários

Um usuário não poderá visualizar, alterar ou excluir dados financeiros pertencentes a outro usuário sem possuir autorização explícita para isso.

### RN75 — Isolamento entre famílias

Um usuário não poderá acessar informações de uma família da qual não participa.

As permissões devem ser verificadas pelo backend em todas as operações relacionadas à família.

### RN76 — Comunicação segura

A comunicação entre os dispositivos dos usuários e o servidor do Clareza deverá utilizar HTTPS/TLS quando o sistema estiver disponibilizado pela internet.

### RN77 — Proteção das senhas

As senhas dos usuários não devem ser armazenadas em texto puro no banco de dados.

As senhas deverão ser armazenadas utilizando um algoritmo seguro de hash de senha, como Argon2id ou bcrypt.

### RN78 — Dados financeiros sensíveis

Os dados financeiros dos usuários devem receber proteção adequada contra acesso não autorizado.

Além da autenticação e autorização, deverão ser avaliadas medidas de criptografia de dados em repouso e proteção da infraestrutura que hospeda o banco de dados.

### RN79 — Banco de dados não exposto publicamente

O banco de dados não deve ser disponibilizado diretamente para os dispositivos dos usuários.

O acesso ao banco deverá ocorrer por meio do backend do Clareza.

### RN80 — Acesso multiplataforma

O sistema deverá permitir que um usuário autenticado utilize sua conta em diferentes dispositivos, incluindo computadores, celulares e outros dispositivos compatíveis com a aplicação web.

### RN81 — Sessões autenticadas

O sistema deverá possuir um mecanismo seguro para manter a autenticação do usuário entre requisições.

O mecanismo escolhido deverá evitar exposição desnecessária de credenciais ou tokens.

### RN82 — Logout

O usuário deverá poder encerrar sua sessão em um dispositivo.

### RN83 — Múltiplos dispositivos

Um mesmo usuário poderá possuir sessões ativas em mais de um dispositivo, respeitando as políticas de segurança definidas pelo sistema.

### RN84 — Proteção contra acesso indevido

O backend deverá validar autenticação, autorização e pertencimento à família antes de operações que envolvam informações financeiras ou familiares.

### RN85 — Princípio do menor privilégio

Cada usuário deverá possuir somente as permissões necessárias para executar as operações permitidas por sua função.

### RN86 — Segredos da aplicação

Senhas do banco de dados, chaves criptográficas, segredos de autenticação e outras credenciais da aplicação não devem ser armazenados diretamente no código-fonte ou enviados ao repositório público.

Essas informações deverão ser fornecidas por mecanismos apropriados de configuração e gerenciamento de segredos.
