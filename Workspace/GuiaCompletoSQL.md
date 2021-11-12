# Guia Completo de SQL

## Neste Guia Completo de SQL você encontrará todo o conteúdo que precisa para aprender sobre a SQL, linguagem de consulta estruturada utilizada por programadores e DBAs para a execução de consultas e comandos nos principais SGBDs do mercado.



[Guias](http://www.devmedia.com.br/guias/)[Banco de Dados](https://www.devmedia.com.br/guias/banco-de-dados)Guia Completo de SQL

## Introdução

**A linguagem SQL é o recurso mais conhecido por DBAs e programadores para a execução de comandos em bancos de dados relacionais**. É por meio dela que criamos tabelas, colunas, índices, atribuímos permissões a usuários, bem como realizamos consultas a dados. Enfim, é utilizando a SQL que “conversamos” com o banco de dados.

Essa breve descrição já deixa clara a importância da compreensão e domínio sobre tal recurso. Saiba, também, que ela facilita não apenas a execução de tarefas em SGBDs, mas também o diálogo entre profissionais de banco de dados e programadores.

Com base nisso, **aprender sobre SQL, a Structured Query Language**, passa a ser fundamental para qualquer um que deseja atuar nessas áreas.

[![Eu preciso aprender SQL?](https://www.devmedia.com.br/arquivos/noticias/devcast/devcast_eu-preciso-aprender-sql_40101.png)DevCastEu preciso aprender SQL?](https://www.devmedia.com.br/eu-preciso-aprender-sql/40101)

### Primeiros passos

Após **conhecer a SQL** é natural nos interessarmos por vê-la na prática, executar os primeiros comandos, as primeiras consultas (queries), mesmo em um banco de dados de exemplo fornecido pelo Sistema Gerenciador de Banco de dados. Mas, por onde começar exatamente? Onde executar esses comandos?

Para responder a essas questões, configurar o ambiente, criar a primeira tabela, inserir dados, assim como executar algumas consultas, preparamos o curso:

[![Curso Primeiros passos com SQL](https://arquivo.devmedia.com.br/cursos/imagem/curso_1895.jpg)CursoCurso de SQL](https://www.devmedia.com.br/curso/curso-de-sql/1895)

Além desse curso, também indicamos os posts a seguir, que apresentam, de forma objetiva, os principais recursos dessa linguagem:

- Artigo

  SQL Básico

- Artigo

  SQL Avançado

### Organização da SQL

No curso acima você deve ter notado que a **linguagem SQL é organizada em subconjuntos**, cada um com propósitos bem definidos (**Figura 1**):

- **DQL - Linguagem de Consulta de Dados -** Define o comando utilizado para que possamos consultar (SELECT) os dados armazenados no banco;
- **DML - Linguagem de Manipulação de Dados -** Define os comandos utilizados para manipulação de dados no banco (INSERT, UPDATE e DELETE);
- **DDL - Linguagem de Definição de Dados -** Define os comandos utilizados para criação (CREATE) de tabelas, views, índices, atualização dessas estruturas (ALTER), assim como a remoção (DROP);
- **DCL - Linguagem de Controle de Dados -** Define os comandos utilizados para controlar o acesso aos dados do banco, adicionando (GRANT) e removendo (REVOKE) permissões de acesso;
- **DTL - Linguagem de Transação de Dados -** Define os comandos utilizados para gerenciar as transações executadas no banco de dados, como iniciar (BEGIN) uma transação, confirmá-la (COMMIT) ou desfazê-la (ROLLBACK).

![Subdivisões da linguagem SQL](https://arquivo.devmedia.com.br/artigos/destaques/guia/SQL.png)Figura 1.Subdivisões da linguagem SQL

Para entendermos melhor, no artigo a seguir vemos uma introdução a linguagem e alguns exemplos dessa subdivisão. Além disso, a partir deste ponto, tomaremos essa subdivisão da SQL para organizar o guia.

- Artigo

  Introdução a linguagem SQL

### SQL DQL - Data Query Language

Como você já pode notar e verificará ao longo da sua carreira, uma das tarefas mais corriqueiras em bancos de dados é a execução de queries. Portanto, saber criá-las da melhor maneira é muito importante para o desempenho do banco e de aplicações que dele dependam.

Neste momento, de primeiros passos, você não precisa se preocupar tanto com isso, mas recomendamos que pense sobre esse assunto a cada novo recurso relacionado a consultas que estudar.

O curso abaixo lhe ensinará a **criar consultas em SQL**, a utilizar o comando **SELECT**. Conheceremos, assim, alguns dos principais recursos, por exemplo: ordenação de registros, funções de agregação, junções, entre outros.

- ![Introdução prática ao comando SQL SELECT](https://www.devmedia.com.br/arquivos/cursos/curso_introducao-pratica-ao-comando-sql-select_2199.png)

  Curso

  Introdução prática ao comando SQL SELECT

- ![Curso SQL: Comando SELECT](https://arquivo.devmedia.com.br/cursos/imagem/curso_consultas-basicas-em-sql_1902.jpg)

  Curso

  Avançando no comando SQL Select

Além desse curso, preparamos mais alguns posts com o intuito de explorar todos os recursos relacionados a consultas em SQL:

- Artigo

  SQL Select: Guia para Iniciantes

- Artigo

  SQL Select: Entendendo a instrução SELECT

- Artigo

  Linguagem SQL: ORDER BY

- Artigo

  Linguagem SQL: Extraindo informação de qualidade com SQL

- Artigo

  SELECT TOP em vários SGBDs

- Curso

  Linguagem SQL: Seleção Múltiplas tabelasNovo!

Para elaborar queries mais específicas, filtrando os dados retornados, assim como fazer cálculos ou verificações de maior ou menor valor, por exemplo, a SQL também nos fornece boas opções.

- ![Documentação WHERE](https://arquivo.devmedia.com.br/noticias/documentacao/documentacao_sql-clausula-where_37645.png)

  Documentação

  WHERE

- ![Documentação Funções de agregação](https://arquivo.devmedia.com.br/noticias/documentacao/documentacao_sql-funcoes-de-agregacao_38463.png)

  Documentação

  Funções de agregação

Além das ferramentas mencionados até aqui, temos ainda outras muito importantes e que influenciam diretamente no tempo da consulta. Portanto, saber utilizá-las é de grande valor para um bom profissional.

#### JOIN

- Artigo

  Entendendo a instrução JOIN

- Artigo

  SQL Join: Entenda como funciona o retorno dos dados

- Artigo

  Merge join cartesian: Uma possível causa de mau desempenho

- Artigo

  Oracle Database: Entendendo o conceito de Join e Outer Join

#### Subqueries

Uma subconsulta (mais conhecida como SUBQUERY ou SUBSELECT) é uma instrução do tipo SELECT dentro de outra instrução SQL. Por serem muito utilizadas em substituição ao JOIN, quando este não atende, seu domínio pelo programador é fundamental. Aprenda!

- ![Um bate papo sobre Subqueries](https://www.devmedia.com.br/arquivos/noticias/devcast/devcast_um-bate-papo-sobre-subqueries_40157.png)

  DevCast

  Um bate papo sobre Subqueries

- ![Trabalhado com subqueries](https://www.devmedia.com.br/arquivos/noticias/artigo/artigo_trabalhado-com-subqueries_40134.png)

  Artigo

  Trabalhado com subqueries

- ![Subqueries: questões de concurso resolvidas](https://www.devmedia.com.br/arquivos/noticias/artigo/artigo_subqueries-questoes-de-concurso-resolvidas_40159.png)

  Artigo

  Subqueries: questões de concurso resolvidas

- ![Avançando com Subqueries](https://www.devmedia.com.br/arquivos/cursos/curso_avancando-com-subqueries_2231.png)

  Curso

  Avançando com Subqueries

#### UNION

- Artigo

  Produto cartesiano e UNION no Oracle

- Artigo

  SQL: Utilizando o Operador UNION e UNION ALL

### SQL DML - Data Manipulation Language

Para a manipulação de dados no banco, temos como comandos da linguagem SQL o INSERT, UPDATE e DELETE, os quais inserem, atualizam e removem dados, respectivamente. Vejamos, nos posts a seguir, como utilizá-los:

- Artigo

  Principais instruções em SQL

- Artigo

  Comandos básicos em SQL - insert, update, delete e select

- Um banco de dados pode armazenar preços, datas e muitas outras informações relevantes para o negócio. Uma vez que o menor fragmento de dado pode influenciar uma decisão, por que excluí-los? Assista aqui um bate papo sobre o uso do comando delete.”

- ![Como você tem usado o comando delete](https://www.devmedia.com.br/arquivos/noticias/devcast/devcast_como-voce-tem-usado-o-comando-delete_39740.png)

- DevCast

- Como você tem usado o comando delete?

- Artigo

  User Defined Functions: Trabalhando com SQL UDFs

### SQL DDL - Data Definition Language

Uma tarefa indispensável, porém realizada com menos frequência, é criar o banco de dados. Em seguida, é natural a criação das tabelas a ele relacionadas. Para isso, lidamos com os comandos CREATE DATABASE e CREATE TABLE da SQL, os quais serão presentados nos posts a seguir, organizados de acordo com o SGBD:

#### SQL Server

- Artigo

  Criando banco de dados e tabelas no SQL Server

- Artigo

  Criando tabelas usando o SQL Server Management Studio

- Artigo

  Criando relacionamentos entre tabelas: Database Diagrams - SQL Server

- Artigo

  Criando banco e tabelas - Garantindo a integridade dos dados

#### Oracle

- Artigo

  Criando e Alterando Tabelas no Oracle

- Artigo

  Utilizando a instrução ALTER TABLE no Oracle

#### MySQL

- Artigo

  Introdução ao MySQL

#### PostgreSQL

- Artigo

  Introdução ao PostgreSQL

- Artigo

  Trabalhando com tabelas no Oracle, MySQL e SQL Server

Ao criar a primeira tabela, logo estaremos em contato com o conceito de chave primária. Esse é um campo obrigatório e representa o identificador único de cada registro presente em uma tabela. E quando temos mais de uma tabela e precisamos estabelecer um relacionamento entre elas, vamos nos deparar com o conceito de chave estrangeira.

Para aprender sobre as chaves primária e estrangeira, acesse:

- Artigo

  SQL: Aprenda a usar a chave primária e a estrangeira

- Artigo

  SQL Server: Boas práticas com chaves

### Índices

Um índice é um recurso muito importante quando o assunto é banco de dados. De forma simples, podemos descrevê-lo como uma estrutura que visa reduzir o tempo gasto com consultas ao banco, facilitando a busca pelos registros desejados.

Para simplificar a compreensão do que é um índice, façamos um paralelo. Considere dois livros: um com um índice e outro sem. Em qual deles você terá mais facilidade e gastará menos tempo para encontrar o assunto que deseja. Do mesmo modo, isso acontece com as tabelas de um banco de dados.

Para aprender sobre índices, como criá-los com a linguagem SQL (DDL), entre outros assuntos relacionados, acesse os posts abaixo:

- Artigo

  Melhoria de desempenho utilizando estatísticas e índices

- Artigo

  Índices no SQL Server

- Artigo

  Dominando os índices columnstore

Com o intuito de atender a diferentes cenários, existem diferentes tipos de índices. Um dos mais recentes e que tem sido bastante utilizado para aprimorar buscas textuais são os índices Full Text. Conheça alguns dos tipos de índices nos posts a seguir:

- Curso

  Full text Search no SQL Server: Buscas Textuais

- Artigo

  Índices Clusterizados e não clusterizados no SQL Server

- Artigo

  Índices colunares no SQL Server

- Artigo

  Desvendando o SQL Server Columnstore Index

- Artigo

  SQL Server 2012 ColumnStore Index

- Artigo

  Otimizando consultas no Oracle utilizando índices

- Artigo

  Trabalhando com Índices no PostgreSQL

Saiba, também, que os índices devem ser utilizados com moderação. Em excesso, provavelmente mais prejudicarão do que ajudarão no desempenho como um todo. Voltando à analogia feita com o livro. Imagine se uma editora optasse por entregar esse livro com dois ou três tipos diferentes de índices. Quão trabalhoso seria para ela, a editora, manter esses índices atualizados a cada mudança no livro. Agora, imagine esse cenário em um banco de dados que é atualizado diariamente...

No post abaixo, aprenda sobre algumas boas práticas para trabalhar com índices:

- Artigo

  Melhores práticas para se trabalhar com índices

Para encerrar esta seção, observe o índice localizado à direita nesta página. Quanto tempo você levaria para encontrar os conteúdos sobre Data Query Language sem esse recurso? Índices são fundamentais para um bom desempenho em qualquer cenário que envolva uma grande quantidade de dados. Portanto, saber como trabalhar com eles é, também, fundamental, para o sucesso da sua carreira como DBA.

### Views

Uma view é definida por muitos como uma "tabela virtual" que agrupa os dados desejados selecionados a partir de alguma consulta.

Normalmente criamos views para facilitar a manipulação dos dados, pois com elas podemos inserir em uma "tabela", isto é, em uma visão, os dados que são necessários para algum tipo de processamento, reduzindo a complexidade na construção de consultas e, ao mesmo tempo, protegendo dados.

Para se aprofundar neste assunto, acesse os posts abaixo:

- Artigo

  Introdução a Views

#### SQL Server

- Artigo

  T-SQL e a utilização de Views

- Artigo

  Conceitos e criação de views no SQL Server

- Artigo

  Views de gerenciamento dinâmico (DMV) no SQL Server

- Artigo

  SQL Server: Turbine suas queries com indexed views

#### Oracle

- Artigo

  Criando Visões (Views) no Oracle

- Artigo

  Oracle Materialized Views: Gerenciando em standby lógicos no Oracle

#### MySQL

- Artigo

  MYSQL – Trabalhando com Views

#### PostgreSQL

- Artigo

  Como funcionam as Views no PostgreSQL

### SQL Procedural

A manipulação de dados armazenados em um banco de dados vai muito além das operações básicas de inserção, consulta e remoção. A administração do banco, por sua vez, também não se limita a criar tabelas e seus relacionamentos, bem como a execução de backups e tarefas relacionadas.

Ao avançar os estudos na área de banco de dados logo verificamos que o conjunto de comandos disponibilizado pela Structured Query Language, ou SQL, tem uma proposta muito bem delineada, mas não atende a todas as necessidades relacionadas a esse contexto, ao artefato que torna organizado e acessível um dos principais ativos de qualquer corporação, seus dados.

Diante disso, é comum que os principais players de banco de dados, a exemplo da Microsoft e da Oracle, oferecem extensões à tão respeitada linguagem de consulta. A Microsoft, com a T-SQL, e a Oracle, com a PL/SQL, fornecem, aos profissionais de banco de dados, um rico conjunto de recursos que possibilitam a criação dos mais específicos procedimentos para atender às demandas de qualquer tipo de aplicação.

Com elas, torna-se possível não apenas a manipulação dos dados, mas também o processamento dos mesmos, declarando, para isso, variáveis, estruturas de condição, repetição, procedimentos, funções, entre outros recursos.

Para cobrir esse assunto, essas extensões, categorizadas como Linguagens Procedurais, preparamos os tópicos a seguir. Neles você encontrará todo o conteúdo de referência, já publicado na DevMedia, para aprender, principalmente, sobre Transact-SQL e Procedural Language/SQL.

### Transact-SQL - T-SQL

A SQL é a linguagem padrão para lidarmos com bancos de dados. No entanto, alguns SGBDs optaram por criar variações dessa para prover novas funcionalidades ou aprimorar recursos oferecidos pela Structured Query Language. A T-SQL, por exemplo, foi criada pela Microsoft em conjunto com a Sybase.

Nesta seção reunimos alguns posts que apresentam recursos da linguagem T-SQL.

#### Recursos básicos

- Artigo

  Introdução ao T-SQL

- Artigo

  Trabalhando com a linguagem T-SQL

- Artigo

  Transact-SQL

- Artigo

  T-SQL no SQL Server 2012: Novidades

- Artigo

  Novos recursos na T-SQL e no uso de índices no SQL Server 2012

- Artigo

  Transact-SQL: T-SQL e a cláusula TOP

- Artigo

  Trabalhando com expressões CASE e a Função IIF no T-SQL

- Artigo

  Trabalhando com Whiles e IFs no T-SQL

- Artigo

  Dynamic T-SQL: Trabalhando com operadores de concatenação(shortcodes)

- Artigo

  Carregando o conteúdo de arquivos XML em tabelas do SQL Server com T-SQL

#### Subqueries

- Artigo

  T-SQL Subqueries: Onde e quando utilizar

- Artigo

  Transact-SQL: Utilizando Subconsultas no T-SQL

- Artigo

  T-SQL e a utilização de Subconsultas correlacionadas

#### Tabelas temporárias

- Artigo

  T-SQL e a utilização de tabelas temporárias

#### Boas práticas

- Artigo

  T-SQL: Aumentando a eficiência do código

- Artigo

  T-SQL: análises e otimizações de CPU

- Artigo

  Boas práticas com T-SQL – Manipulando Datas

### PL/SQL

Além da T-SQL, outra opção bastante conhecida é a PL/SQL, ou Procedural Language/Structured Query Language. Criada pela Oracle, essa linguagem também oferece diferentes recursos para lidarmos com bancos de dados dessa empresa, assim como elaborar procedimentos que processarão dados para outras soluções Oracle. Abaixo, listamos um curso e alguns posts relacionados:

- Curso

  PL/SQL Oracle

- Artigo

  Conhecendo o PL/SQL

- Artigo

  PL/SQL Placeholders: Como definir o tipo de dados

- Artigo

  PL/SQL: Tipos de dados Escalar e LOB

- Artigo

  Como criar scripts DDL com PL/SQL: Conceitos e aplicações práticas

- Artigo

  Trabalhando com Packages PL/SQL

- Artigo

  PL/SQL Collections

- Artigo

  Oracle PLSQL: Cursores

Assim como em qualquer linguagem, o tratamento de exceções também recebeu atenção especial da Oracle. Para aprender sobre o tratamento de exceções com a PL/SQL, acesse:

- Artigo

  Tratamento de Exceções de Sistema na linguagem PL/SQL

- Artigo

  Recursos Avançados para o Tratamento de Exceções na Linguagem PL/SQL

### Stored Procedures

Um stored procedure, ou procedimento armazenado, é um recurso que permite a execução de lógica dentro do banco de dados, de forma semelhante a um método ou função no código de uma aplicação, mas escrito com a linguagem do banco de dados.

Assim, ao invés de deixar toda a lógica na aplicação cliente, podemos trazer parte dela para o servidor de banco de dados, o que, em muitos casos, pode trazer ganhos de performance, ao evitar que dados precisem ser enviados pela rede para serem processados.

A criação de stored procedures é definida pelo comando CREATE PROCEDURE e sua remoção, com o DROP PROCEDURE.

Para aprender sobre este assunto, acesse:

- Artigo

  Como trabalhar com Stored Procedures e Cursores no Oracle, SQL Server e Firebird e PostgreSQL

- Artigo

  Vantagens da utilização de stored procedures em aplicativos web para melhoria de performance e segurança

Para se aprofundar em stored procedure, confira as publicações a seguir, organizadas de acordo com o banco de dados:

#### SQL Server

- Artigo

  Introdução aos Stored Procedures no SQL Server

- Artigo

  Stored Procedures no SQL Server 2008

- Artigo

  SQL Dinâmico com Stored Procedure

- Artigo

  Trabalhando com System Stored Procedures

- Artigo

  Algoritmo de roteamento em stored procedure no SQL Server 2005

#### Oracle

- Artigo

  PL/SQL Functions e Procedures

- Artigo

  Stored Procedures, Functions e Packages em bancos de dados Oracle

- Artigo

  Estrutura - Stored Procedures em PL/SQL

- Artigo

  Criando Procedure - Stored Procedures em PL/SQL

- Artigo

  Parâmetro de entrada - Stored Procedures em PL/SQL

- Artigo

  Parâmetro de saída - Stored Procedures em PL/SQL

#### MySQL

- Artigo

  Stored Procedures no MySQL

- Artigo

  Utilizando Stored Procedures, Views e Triggers no MySQL

- Artigo

  MySQL Stored Procedures

#### PostgreSQL

- Artigo

  Trabalhando com Stored Procedures no PostgreSQL

#### Firebird

- Artigo

  Usando Stored Procedures com Firebird e InterBase

### Triggers

Um trigger, ou gatilho, é um tipo de stored procedure que é chamado toda vez que um evento a ele relacionado acontece, por exemplo: a atualização de um dado no banco, a modificação da estrutura de uma tabela, entre outros.

Assim, é comum utilizar triggers quando o objetivo é manter a consistência de dados ou fazer alguma auditoria, por exemplo. O uso desse recurso é observado em códigos de definição (DDL) e manipulação (DML) de dados.

Para criar um trigger, iniciamos a declaração com o comando CREATE TRIGGER, e para remoção, DROP TRIGGER.

Uma introdução mais detalhada sobre este assunto pode ser obtida nos posts:

- Artigo

  Introdução a Triggers

- Artigo

  Desvendando as Triggers

- Artigo

  Desenvolvendo Triggers em SQL Server, Oracle, Firebird e Postgres

- DevCast

  Você usa Triggers?

Nas publicações abaixo, organizadas de acordo com o banco de dados, exploramos situações mais específicas, que podem atender às demandas do seu dia a dia. Confira:

#### SQL Server

- Artigo

  Triggers no SQL Server: teoria e prática aplicada em uma situação real

- DevCast

  Update Triggers: Como proteger tabelas de SQL Injection

- Artigo

  Trabalhando com Trigger de Logon no SQL Server

- Artigo

  Usando CDC e Trigger em Auditorias no SQL Server 2008

#### Oracle

- Artigo

  Triggers PL/SQL: saiba quando e por que usar

#### Firebird

- Artigo

  Trabalhando com Triggers DML no Oracle

- Artigo

  SQL Triggers: É possível desabilitar uma trigger por usuário?

- Artigo

  Automação de Trigger de Auto Incremento: Oracle

#### MySQL

- Artigo

  MySQL Básico: Triggers

- Artigo

  MySQL – Triggers

- Artigo

  Implementando controle de estoque no MySQL com triggers e procedures

#### PostgreSQL

- Artigo

  Trabalhando com triggers no PostgreSQL

#### Firebird

- Artigo

  Explorando Triggers no Firebird

### Desempenho e segurança

Ao gerenciar bancos de dados estamos tratando de um recurso de grande importância a qualquer instituição: seus dados. Devemos, portanto, prezar pela segurança e pelo desempenho de acesso aos mesmos.

Para tratar de desempenho, indicamos o post:

- Artigo

  Otimização de Consultas SQL

Com relação à segurança, entre tantos assuntos, destacamos aqui, principalmente, o SQL Injection. Aprenda o que é e como evitar acessando os links abaixo:

- Artigo

  SQL Injection: o que é, por que funciona e como prevenir

- DevCast

  Evitando SQL Injection em Aplicações Web - Testes com SQLMAP

- Artigo

  SQL Injection em múltiplas plataformas

- Artigo

  Prevenindo SQL Injection

### Documentação

Com o intuito de apoiar seu aprendizado sobre a linguagem SQL, elaboramos algumas documentações que você pode acessar para consulta rápida.

Na documentação a seguir temos um apanhado dos principais tópicos da DML e da DQL:

[![Documentação SQL](https://arquivo.devmedia.com.br/cursos/SQL_2199/doc_SQL.png)ProjetoDocumentação SQL](https://www.devmedia.com.br/exemplo/documentacao-sql/76)

Nestes posts você aprenderá não apenas sobre a sintaxe dos comandos, mas também como utilizá-los da melhor maneira para obter o resultado desejado.

- Documentação

  WHERE

- Documentação

  Funções de agregação

- Documentação

  JOIN

### Conteúdo extra

A qualidade do código não é uma preocupação exclusiva a programadores. Ao criar qualquer comando a ser executado no banco de dados também devemos levar em consideração essa questão.

Lembre-se que esse código provavelmente passará por manutenção e essa tarefa pode ser atribuída a outras pessoas. Para lhe ajudar a escrever um código SQL mais fácil de ler e manter, recomendamos o post:

[![Artigo Linguagem SQL: torne seu código SQL mais legível](https://arquivo.devmedia.com.br/noticias/artigos/artigo_linguagem-sql-torne-seu-codigo-sql-mais-legivel_38062.jpg)ArtigoLinguagem SQL: torne seu código SQL mais legível](https://www.devmedia.com.br/linguagem-sql-torne-seu-codigo-sql-mais-legivel/38062)

Por fim, saiba que essa linguagem também é aplicada em diferentes contextos, não ficando restrita ao acesso a dados persistidos em bancos relacionais. Isso nos mostra que seu aprendizado é útil e aplicável a outros cenários, como pesquisa semântica em documentos ou mesmo consultas a bases de dados não relacionais.

- Artigo

  Utilizando a linguagem SQL em diferentes contextos

### Mais sobre bancos de dados

Além dos cursos e exemplos sugeridos nesse guia, a DevMedia publica com frequência novos conteúdos sobre bancos de dados, os quais você pode conferir nos Guias de Consulta abaixo:

- Guia de Carreira

  Banco de dados para ProgramadoresGuia

- Guia de consulta

  Modelagem de dadosGuia

- Guia de consulta

  SQL ServerGuia

- Guia de Consulta

  MySQLGuia

- Guia de Consulta

  PostgreSQLGuia

- Guia de Consulta

  OracleGuia

- Guia de Consulta

  NoSQL e MongoDBGuia

Você também pode conferir todo o conteúdo de banco de dados [clicando aqui](https://www.devmedia.com.br/busca/?txtsearch=&tipo=0&site=37).