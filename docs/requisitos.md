# Clareza — Documento de Requisitos

**Versão:** 1.1
**Projeto:** Clareza
**Tipo:** Projeto de estudo e portfólio

---

# 1. Visão do Projeto

O **Clareza** é um sistema web de controle financeiro pessoal e familiar.

O sistema permitirá que usuários registrem manualmente suas movimentações financeiras, como:

* Entradas;
* Despesas;
* Investimentos.

A aplicação também permitirá visualizar essas informações por meio de dashboards, tabelas e gráficos.

Além do controle individual, usuários poderão participar de uma **família financeira**, permitindo uma visão agregada da situação financeira do grupo.

O sistema também terá um indicador chamado **Saúde da Família**, responsável por analisar o comportamento financeiro da família ao longo do tempo e apresentar uma interpretação da situação financeira atual.

A primeira versão será desenvolvida com foco em aprendizado e portfólio, sem integrações bancárias automáticas ou recursos de inteligência artificial.

---

# 2. Objetivo

O objetivo do Clareza é permitir que uma pessoa consiga:

* Registrar sua renda;
* Registrar suas despesas;
* Registrar seus investimentos;
* Organizar suas movimentações por categorias;
* Visualizar sua situação financeira;
* Acompanhar sua evolução financeira ao longo do tempo;
* Participar de uma família financeira;
* Acompanhar a situação financeira agregada da família;
* Identificar possíveis problemas ou melhorias na saúde financeira familiar.

---

# 3. Público-Alvo

O sistema será direcionado principalmente para:

* Pessoas que desejam controlar suas finanças;
* Pessoas que desejam acompanhar investimentos manualmente;
* Famílias que desejam acompanhar sua situação financeira em conjunto;
* Pessoas que desejam compreender melhor seus hábitos financeiros.

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
* Categorias predefinidas;
* Categorias personalizadas;
* Dashboard individual;
* Dashboard familiar;
* Indicador de Saúde da Família;
* Controle de permissões;
* API REST;
* Banco de dados MySQL.

---

# 5. Usuários

Cada pessoa deverá possuir uma conta de usuário.

Um usuário poderá pertencer a **apenas uma família por vez**.

Um usuário que não pertence a nenhuma família poderá:

* Criar uma família;
* Solicitar entrada em uma família existente através do código da família.

Um usuário que já pertence a uma família não poderá solicitar entrada em outra família enquanto permanecer na família atual.

Caso saia da família, poderá posteriormente criar ou solicitar entrada em outra.

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

Uma família também poderá existir sem membros além do usuário que a criou, dependendo das regras de criação.

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
4. O sistema cria uma solicitação de entrada;
5. A solicitação fica com status `PENDENTE`;
6. Um administrador da família visualiza a solicitação;
7. O administrador pode aceitar ou rejeitar;
8. Caso aceite, o usuário passa a fazer parte da família;
9. Caso rejeite, o usuário não entra na família.

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

Isso permite que a responsabilidade administrativa seja transferida para outro membro caso necessário.

---

# 13. Chefe da Família

O chefe da família representa um papel relacionado à visualização financeira dos membros.

Uma família poderá possuir:

* Zero chefes;
* Um chefe.

Não será permitido possuir mais de um chefe simultaneamente.

O chefe poderá visualizar os dados financeiros individuais dos membros da própria família.

O chefe nunca poderá visualizar dados financeiros de usuários que não pertencem à sua família.

O papel de chefe não determina quem administra as solicitações de entrada.

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
* O usuário não terá mais acesso às informações familiares;
* O usuário não poderá mais visualizar a Saúde da Família;
* O usuário continuará possuindo sua própria conta.

Após sair, poderá criar uma nova família ou solicitar entrada em outra.

---

# 16. Saída do Administrador

Caso um administrador deixe a família, outro administrador deverá assumir suas responsabilidades.

O sistema não deverá permitir que uma família fique sem administrador caso existam solicitações ou ações administrativas pendentes.

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

---

# 21. Despesas

Despesas representam valores gastos pelo usuário.

Exemplos:

* Alimentação;
* Moradia;
* Transporte;
* Saúde;
* Educação;
* Lazer.

Uma despesa deverá possuir pelo menos:

* Valor;
* Data;
* Descrição;
* Categoria.

---

# 22. Investimentos

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

# 23. Categorias

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

As categorias personalizadas deverão estar associadas ao usuário ou a uma estrutura definida pelo sistema para evitar conflitos entre usuários.

---

# 24. Dashboard Individual

Cada usuário terá acesso a um dashboard individual.

O dashboard poderá apresentar:

* Saldo atual;
* Total de entradas;
* Total de despesas;
* Total de investimentos;
* Gastos por categoria;
* Evolução mensal;
* Comparação entre entradas e despesas;
* Capacidade de poupança/investimento.

Os dados deverão ser apresentados por meio de:

* Cards;
* Tabelas;
* Gráficos;
* Indicadores.

---

# 25. Dashboard Familiar

A família possuirá uma visão financeira agregada.

O dashboard familiar poderá apresentar:

* Total de entradas da família;
* Total de despesas;
* Total de investimentos;
* Evolução financeira;
* Gastos por categoria;
* Comparação entre entradas e despesas;
* Capacidade de poupança/investimento da família.

Membros comuns não deverão visualizar os valores individuais dos outros membros.

O chefe poderá visualizar informações financeiras individuais dos membros da própria família.

---

# 26. Saúde da Família

O Clareza possuirá um indicador chamado **Saúde da Família**.

Esse indicador não será apenas um status fixo como:

* Saudável;
* Ruim;
* Crítica.

O objetivo é criar uma interpretação mais dinâmica da situação financeira da família.

Exemplos de estados possíveis:

* Saúde de ferro;
* Saudável;
* Se recuperando;
* Sentindo uma dor de cabeça;
* Sentindo sintomas ruins;
* Enxaqueca pesada.

Os nomes e regras exatas desses estados poderão ser modificados durante o desenvolvimento.

---

# 27. Análise da Saúde da Família

A Saúde da Família deverá considerar o comportamento financeiro ao longo do tempo.

Entre os indicadores que poderão ser analisados estão:

* Relação entre entradas e despesas;
* Crescimento das despesas;
* Crescimento das entradas;
* Capacidade de poupança;
* Evolução dos investimentos;
* Variação dos gastos;
* Gastos por categoria;
* Tendências financeiras dos últimos meses.

A análise deverá considerar tendências e não apenas uma fotografia de um único mês.

Exemplo:

Se durante vários meses as despesas aumentarem enquanto a capacidade de poupança diminuir, a Saúde da Família poderá apresentar uma interpretação mais negativa.

Caso a família esteja reduzindo despesas e aumentando sua capacidade de poupança ao longo dos meses, a interpretação poderá melhorar.

---

# 28. Explicação da Saúde

A Saúde da Família não deverá apresentar somente um estado.

Sempre que possível, o sistema deverá informar os motivos que contribuíram para aquele estado.

Exemplo conceitual:

```text
Saúde da Família:
Sentindo uma dor de cabeça

Motivos:
- Despesas aumentaram nos últimos 3 meses;
- Capacidade de poupança diminuiu;
- Entradas permaneceram estáveis.

Período analisado:
Últimos 3 meses
```

Isso permitirá que o usuário compreenda por que determinado estado foi apresentado.

---

# 29. Histórico da Saúde

Futuramente, o sistema poderá armazenar o histórico das análises da Saúde da Família.

Isso permitirá identificar a evolução da situação financeira.

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

Essa funcionalidade poderá ser implementada posteriormente caso não seja necessária para a primeira versão.

---

# 30. API REST

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
* Comunicação com o banco de dados.

---

# 31. Autenticação

O sistema deverá possuir autenticação de usuários.

O usuário deverá realizar login para acessar seus dados.

As senhas:

* Não deverão ser armazenadas em texto puro;
* Deverão utilizar hash seguro;
* Não deverão ser retornadas pela API.

---

# 32. Autorização

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

# 33. Requisitos Funcionais

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

### RF20 — Listar movimentações

O sistema deverá permitir a visualização das movimentações do usuário.

### RF21 — Editar movimentação

O sistema deverá permitir a edição de movimentações do usuário.

### RF22 — Excluir movimentação

O sistema deverá permitir a exclusão de movimentações do usuário.

### RF23 — Categorias

O sistema deverá disponibilizar categorias predefinidas.

### RF24 — Categorias personalizadas

O sistema deverá permitir a criação de categorias personalizadas.

### RF25 — Dashboard individual

O sistema deverá apresentar informações financeiras individuais do usuário.

### RF26 — Dashboard familiar

O sistema deverá apresentar informações financeiras agregadas da família.

### RF27 — Saúde da Família

O sistema deverá calcular e apresentar a Saúde da Família.

### RF28 — Análise financeira

O sistema deverá analisar indicadores financeiros ao longo do tempo.

### RF29 — Controle de acesso

O sistema deverá impedir o acesso a recursos não autorizados.

### RF30 — Dados financeiros do chefe

O sistema deverá permitir que o chefe visualize os dados financeiros individuais dos membros da própria família.

---

# 34. Requisitos Não Funcionais

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

O sistema deverá impedir inconsistências relacionadas à participação dos usuários nas famílias e às permissões de acesso.

---

# 35. Fora do Escopo da Primeira Versão

Os seguintes recursos não farão parte da primeira versão:

* Open Banking;
* Integração automática com bancos;
* Importação automática de extratos;
* Inteligência Artificial;
* Integração com corretoras;
* Rentabilidade automática de investimentos;
* PIX;
* Pagamentos;
* Cartão de crédito integrado;
* Aplicativo mobile nativo.

Esses recursos poderão ser considerados em versões futuras.

---

# 36. Possíveis Evoluções

Após a conclusão da primeira versão, poderão ser adicionados:

* Open Banking;
* Integração com bancos;
* Inteligência Artificial;
* Análise financeira avançada;
* Recomendações personalizadas;
* Alertas financeiros;
* Metas financeiras;
* Orçamentos;
* Investimentos integrados;
* Aplicativo mobile;
* Histórico avançado da Saúde da Família;
* Mais níveis de permissão;
* Notificações.

---

# 37. Princípio de Desenvolvimento

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

---

# 38. Tecnologias

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

# 39. Arquitetura Inicial

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

# 40. Status do Projeto

O projeto encontra-se na fase de **levantamento e definição de requisitos**.

Próximas etapas:

1. Revisar requisitos;
2. Definir regras de negócio;
3. Identificar entidades;
4. Definir relacionamentos;
5. Criar modelo entidade-relacionamento;
6. Criar banco de dados MySQL;
7. Criar backend;
8. Criar API REST;
9. Implementar autenticação e autorização;
10. Criar frontend;
11. Implementar dashboards;
12. Implementar Saúde da Família;
13. Testar e documentar o sistema.
