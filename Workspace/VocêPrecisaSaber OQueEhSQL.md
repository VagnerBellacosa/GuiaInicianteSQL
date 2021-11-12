# Você precisa saber o que é SQL!

[Gustavo Furtado de Oliveira Alves](https://dicasdeprogramacao.com.br/autor/gustavo-furtado-de-oliveira-alves/)[Banco de dados](https://dicasdeprogramacao.com.br/categoria/banco-de-dados/)[35 Comentários](https://dicasdeprogramacao.com.br/o-que-e-sql/#disqus_thread)

**SQL** (Structured Query Language) é a linguagem padrão universal para manipular [bancos de dados](http://www.dicasdeprogramacao.com.br/o-que-e-um-banco-de-dados/) relacionais através dos [SGBDs](http://www.dicasdeprogramacao.com.br/o-que-e-um-sgbd/). Isso significa que todos os SGBDRs (Sistema de Gerenciamento de Banco de Dados Relacionais) oferecem uma interface para acessar o banco de dados utilizando a linguagem SQL, embora com algumas variações. Logo, saber o que é SQL e como utilizá-la é fundamental para qualquer desenvolvedor de softwares.

A "Linguagem Estruturada de Consultas" (SQL, traduzida para o português) é utilizada para interagir com o SGBD e executar várias tarefas como inserir e alterar registros, criar objetos no banco de dados, gerenciar usuário, consultar informações, controlar transações, etc. Todas as operações realizadas no banco de dados podem ser solicitadas ao SGBD utilizando esta linguagem.

![SQL](https://dicasdeprogramacao.com.br/images/o-que-e-sql/SQL.png)

ad

A linguagem SQL é dividida em 4 agrupamentos de acordo com o tipo de operação a ser executada no banco de dados. A saber, DML (Data Manipulation Language, ou Linguagem de Manipulação de Dados e português), DDL (Data Definition Language, ou Linguagem de Definição de Dados em português), DCL (Data Control Language, ou Linguagem de Controle de Dados em português) e DTL (Data Transaction Language, ou Linguagem de Transação de Dados em português).

Alguns autores classificam também uma divisão da linguagem para consultas, a DQL (Data Query Language, Linguagem de Consulta de Dados), que tem apenas um comando (SELECT), porém é mais comum encontrar este comando como integrante da DML, juntamente com os comandos INSERT, UPDATE e DELETE. Vejamos os comandos SQL de cada agrupamento.

## DML - DATA MANIPULATION LANGUAGE

**DML** (Linguagem de Manipulação de Dados) é o subconjunto mais utilizado da linguagem **SQL**, pois é através da DML que operamos sobre os dados dos bancos de dados com instruções de inserção, atualização, exclusão e consulta de informações. Os comandos SQL desse subconjunto são:

- INSERT

  : utilizado para inserir registros (tuplas), em uma tabela.

  - Exemplo: INSERT into CLIENTE(ID, NOME) values(1,'José');

- UPDATE

  : utilizado para alterar valores de uma ou mais linhas (tuplas) de uma tabela.

  - Exemplo: UPDATE CLIENTE set NOME = 'João'  WHERE ID = 1;

- DELETE

  : utilizado para excluir um ou mais registros (tupla) de uma tabela.

  - Exemplo: DELETE FROM CLIENTE WHERE ID = 1;

- SELECT

  : O principal comando da SQL, o comando select é utilizado para efetuar consultas no banco de dados.

  - Exemplo: SELECT ID, NOME FROM CLIENTE;

ad

> Nota: **Registro**, **Linha** e **Tupla** são palavras sinônimas para referenciar a uma linha da tabela.

## DDL - DATA DEFINITION LANGUAGE

**DDL** (Linguagem de Definição de Dados) é o subconjunto da **SQL** utilizado para gerenciar a estrutura do banco de dados. Com a DDL podemos criar, alterar e remover objetos (tabelas, visões, funções, etc.) no banco de dados. Os comandos deste subconjunto são:

- **CREATE**: utilizado para criar objetos no banco de dados.

  - Exemplo (criar uma tabela): CREATE TABLE CLIENTE ( ID INT PRIMARY KEY, NOME VARCHAR(50));

- ALTER

  : utilizado para alterar a estrutura de um objeto.

  - Exemplo (adicionar uma coluna em uma tabela existente): ALTER TABLE CLIENTE ADD SEXO CHAR(1);

- DROP

  : utilizado para remover um objeto do banco de dados.

  - Exemplo (remover uma tabela): DROP TABLE CLIENTE;

## DCL - DATA CONTROL LANGUAGE

ad

**DCL** (Linguagem de Controle de Dados) é o subconjunto da **SQL** utilizado para controlar o acesso aos dados, basicamente com dois comandos que permite ou bloqueia o acesso de usuários a dados. Vejamos estes comandos:

- GRANT

  : Autoriza um usuário a executar alguma operação.

  - Exemplo (dar permissão de consulta na tabela cliente para o usuário carlos): GRANT select ON cliente TO carlos;

- REVOKE

  : Restringe ou remove a permissão de um usuário executar alguma operação.

  - Exemplo (não permitir que o usuário carlos crie tabelas no banco de dados): REVOKE CREATE TABLE FROM carlos;

## DTL - DATA TRANSACTION LANGUAGE

**DTL** (Linguagem de controle de transações) é o subconjunto da **SQL** que fornece mecanismos para controlar transações no banco de dados. São 3 comandos: iniciar uma transação (BEGIN TRANSACTION), efetivar as alterações no banco de dados (COMMIT) e cancelar as alterações (ROLLBACK).

## CONCLUSÃO

ad

Quem quer trabalhar com desenvolvimento de softwares precisa aprender a SQL, pois a maioria dos sistemas de informação interage com banco de dados, e essa é a linguagem universal para fazer qualquer coisa nos bancos de dados relacionais (o tipo de banco de dados mais utilizado na industria). Pode haver pequenas variações na linguagem dependendo do [SGBD](http://www.dicasdeprogramacao.com.br/o-que-e-um-sgbd/), mas a sintaxe dos comandos são muito parecidas.

Cada comando citado neste artigo possui uma série de recursos, o comando que tem mais recursos, obviamente, é o comando SELECT. O objetivo deste artigo é apenas apresentar a linguagem SQL e seus comandos, continue ligado aqui no **{ Dicas de Programação }** que vamos ver os detalhes de cada comando desta linguagem