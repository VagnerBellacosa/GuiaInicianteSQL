
Artigo[Invista em você! Saiba como a DevMedia pode ajudar sua carreira.](https://www.devmedia.com.br/introducao-a-linguagem-sql/40690#modulo-mvp)

# Introdução a linguagem SQL

## Neste artigo veremos uma introdução ao SQL, que é uma linguagem padrão para uso em banco de dados relacionais.



Por que eu devo ler este artigo:Aprenda neste artigo os **conceitos iniciais da linguagem SQL**, que te permitirão trabalhar com bancos de dados relacionais e sistemas que fazem acesso a eles.

[Voltar](https://www.devmedia.com.br/sql/introducao)Suporte ao alunoAnotarMarcar como concluído

[Artigos](https://www.devmedia.com.br/artigos/)[Banco de Dados](https://www.devmedia.com.br/artigos/banco-de-dados)Introdução a linguagem SQL

**SQL (Structured Query Language) é a linguagem padrão em sistemas de gerenciadores de bancos de dados relacionais**. É por meio dela que criamos tabelas, colunas, índices, garantimos e removemos privilégios a usuários e, principalmente, consultamos os dados armazenados em tabelas.

------

##### Guia do artigo:

- [Como utilizar o SQL?](https://www.devmedia.com.br/introducao-a-linguagem-sql/40690#use)
- [DDL](https://www.devmedia.com.br/introducao-a-linguagem-sql/40690#ddl)
- [DML/DQL](https://www.devmedia.com.br/introducao-a-linguagem-sql/40690#dml)

------

Trata-se de uma linguagem de consulta e de grande importância para desenvolvedores, pois é com ela que podemos nos comunicar com os [bancos de dados](https://www.devmedia.com.br/conceitos-fundamentais-de-banco-de-dados/1649) utilizados pelas aplicações.

### Como utilizar o SQL?

O primeiro passo para **aprender SQL** é entender que essa se trata de uma linguagem declarativa, com a qual nos preocupamos menos em como as coisas são feitas e nosso trabalho passa a ser informar o que queremos fazer combinando um conjunto de comandos disponibilizado por ela. Os comandos da linguagem SQL são divididos em conjuntos e essa separação é feita de acordo com o que cada comando faz. Comandos que fazem atividades similares são agrupados no mesmo conjunto.

Vejamos a seguir quais são esses conjuntos com exemplos que apresentam os seus comandos mais utilizados.

- DDL

   

  \- Data Definition Language - Conjunto de comandos que lidam com os objetos, criando bancos de dados, esquemas, tabelas, campos, etc. Dentre os mais utilizados temos CREATE, ALTER e DROP. Exemplo:

  ```
  CREATE TABLE produto -- Criação da tabela produto
  ```

- DML

   

  \- Data Manipulation Language - Os comandos aqui lidam com os dados. Alguns muito comuns são INSERT, UPDATE e DELETE. Exemplo:

  

  ```
  INSERT INTO produto (nome, valor) VALUES (‘geladeira’,’1200’)
  -- Inserção de valores na tabela produto
  ```

- DQL

   

  \- Data Query Language - Linguagem de consulta de dados conta com o conjunto da instrução utilizada para a obtenção dos registros dos bancos de dados. Exemplo:

  ```
  SELECT nome, valor FROM produto -- Recupera valores da tabela produto
  ```

  Aqui vale destacar que algumas literaturas colocam o SELECT como integrante do grupo DML, já que a sua função nesse grupo seria de recuperação de dados.

Outros conjuntos mais específicos são:

- DTL

   

  \- Data Transaction Language - Linguagem de transação de dados que conta com o conjunto de instruções usadas para gerenciar as transações que ocorrem dentro do banco de dados. Exemplo:

  ```
  BEGIN TRAN -- Inicia uma transação
  ```

- DCL

   

  \- Data Control Language - Linguagem de controle de dados possui o conjunto das instruções usadas para controlar o acesso e gerenciar permissões de usuários no banco de dados. Exemplo:

  ```
  GRANT CREATE TABLE to usuario -- Concede privilégio de criar tabela a um usuário
  ```

Dentre essas subdivisões da linguagem SQL, DDL e DML/DQL contém os comandos mais utilizados. Em DDL temos comandos que cuidam dos objetos que compõem um bancos de dados, tais como o próprio banco, as tabelas e os usuários, entre outros. Em DML estão agrupados os comandos para a manipulação dos dados. Vejamos a seguir mais algumas informações sobre esses conjuntos.

### DDL

Agora veremos alguns comandos do grupo DDL com exemplos de códigos. Nesse momento não é necessário se preocupar em entender completamente cada comando, pois em próximos conteúdos abordaremos cada um deles em mais detalhes.

Os principais comandos agrupados em DDL são:

- CREATE TABLE
- DROP TABLE
- ALTER TABLE
- TRUNCATE

No **Código 1**, podemos ver o uso do comando CREATE TABLE.

```
CREATE TABLE Contato (
    Id int,
    Nome varchar(255),
    Telefone varchar(11)
);
```

**Código 1**. Criação de tabela

Acima, temos a criação de uma tabela no SQL. O comando CREATE TABLE cria uma tabela, que no nosso exemplo, terá o nome Contato. A estrutura interna da tabela é inserida dentro dos parênteses, onde vamos colocar as colunas com as suas propriedades. Nesse caso criamos três colunas, cada uma com um nome e com o seu tipo.

No [curso de SQL](https://www.devmedia.com.br/curso/curso-de-sql/1895), é explicado com mais detalhes a construção e estrutura das tabelas.

Além da criação de tabelas, podemos também excluí-las. E isso pode ser feito com o uso do comando DROP, que vemos no **Código 2**.

```
DROP TABLE Contato
```

**Código 2**. Exclusão de tabela

Com a execução do código acima, excluiremos a tabela Contato. O comando DROP TABLE serve para remover uma tabela existente de um banco de dados.

O comando DROP TABLE remove a tabela e todos os dados contidos nela, então deve ser usado com cautela.

Uma tabela também pode ter sua estrutura alterada. Isso é feito por meio do comando ALTER TABLE. No **Código 3**, temos um exemplo no qual adicionamos uma coluna à tabela Contato.

```
ALTER TABLE Contato
Add email VARCHAR(255)
```

**Código 3**. Alteração de tabela

Mais um comando DDL é o comando TRUNCATE. Com ele, excluímos todos os registros de uma tabela, deixando-a do jeito como foi criada. No **Código 4**, temos o uso desse comando.

```
TRUNCATE TABLE Contato
```

**Código 4**. Remoção dos dados da tabela

### DML/DQL

Os principais comandos agrupados em DML e DQL são:

- INSERT
- DELETE
- UPDATE
- SELECT

Podemos inserir registros numa tabela por meio do comando INSERT. Nele, informamos as colunas que queremos inserir os dados, e os valores que queremos preencher nesses campos da tabela. No **Código 5** vemos como fica o uso dessa instrução:

```
INSERT INTO Cliente(nome, telefone)
VALUES ('Suzana', '99999-9999')
```

**Código 5**. Inserindo dados da tabela

Para a exclusão de registros temos o comando DELETE, que pode ser usado em junção com a cláusula WHERE. Caso essa condição não esteja presente, todos os dados da tabela serão apagados. Veja no **Código 6**

```
DELETE FROM Cliente WHERE id = 2 -- Excluirá apenas o registro com id igual a 2
DELETE FROM Cliente -- Excluirá todos os registros
```

**Código 6**. Deletando dados da tabela

Além de excluir dados, temos casos em que será necessário atualizar valores na tabela. E para isso temos o comando UPDATE que também pode ser usado com a cláusula WHERE. Da mesma forma como acontece com o comando DELETE, se a condição não for usada, todos os dados da tabela serão atualizados.

```
UPDATE Cliente SET telefone = '99999-9999' WHERE id = 3
  -- Atualizará o telefone do cliente com id = 3
UPDATE Cliente SET telefone = '99999-9999'
  -- Atualizará o telefone de todos os cliente
```

**Código 7**. Atualizando dados da tabela

Abaixo temos o uso do comando SELECT, com o qual recuperamos informações de uma tabela. No **Código 8** estamos recuperando as informações nome e telefone da tabela Cliente.

```
SELECT nome, telefone FROM Cliente
```

**Código 8**. Selecionando dados da tabela

Em algumas literaturas DQL é um subconjunto de DML. A partir daqui consideraremos dessa forma.

Com o comando SELECT podemos obter os dados das tabelas que serão selecionados de acordo com a consulta que estamos fazendo. Devemos indicar neste comando a tabela que queremos recuperar os dados, se há alguma condição para os registros serem recuperados, ou se queremos limitar a quantidade de linhas para ser retornada etc. No **Código 9**, vemos como isso acontece na prática.

```
SELECT nome, telefone
FROM Cliente
WHERE nome = 'Ramon'
```

**Código 9**. Delimitando a recuperação de dados

A instrução acima recupera dados de uma tabela com uma condição especificada. O termo SELECT é usado para informar qual o campo deve ser recuperado. Após ele, pode ser colocado um ou mais campos que se deseja obter. Em seguida temos o uso do termo FROM, com o qual indicamos de qual tabela os dados serão obtidos, tendo na sequência o nome da tabela em que faremos essa operação. Por fim, usamos o WHERE para definir uma condição para a obtenção dos dados, que no caso acima define que o campo nome seja igual a “Ramon”.

Um recurso que é usado com frequência é a junção de duas ou mais tabelas. E isso é feito por meio da instrução JOIN. No **Código 10**, temos um exemplo no qual pegamos dados de duas tabelas que pertencem a um registro específico. Nesse momento não se preocupe em entender a junção e todas as variações que ela pode ter, pois abordaremos o JOIN com mais detalhes em outro conteúdo:

```
SELECT c.nome, c.telefone, p.codigo, p.data_pedido
FROM Cliente c
JOIN Pedido p
ON c.id = p.id_cliente
WHERE c.id = 5
```

**Código 10**. Juntando dados de duas tabelas

Nessa consulta estamos obtendo os dados das tabelas Cliente e Pedido, que recebem os apelidos c e p respectivamente. Com esses apelidos acessamos os dados das colunas dessas tabelas. Por exemplo, c.nome é uma coluna da tabela Cliente, assim como p.codigo é uma coluna da tabela Pedido. Por meio do JOIN em conjunto com o ON, trazemos os dados que desejamos no SELECT, especificando qual a coluna que é comum entre as duas tabelas. Por fim, utilizamos o WHERE especificando que a busca deve ser restrita ao id da tabela Cliente.

### Conclusão

Como vimos neste artigo, a linguagem SQL tem grande importância, pois com ela acessamos e manipulamos registros dentro um banco de dados. Armazenar e manipular informações de modo que elas possam ser usadas nas aplicações são habilidades relevantes para um programador. O **uso da linguagem SQL** ajuda na criação de diversas soluções, desde sistemas simples até os mais complexos.

#### Confira também

[![SQL](https://arquivo.devmedia.com.br/cursos/imagem/curso_1895.jpg)SQLMatéria](https://www.devmedia.com.br/carreira/banco-de-dados/sql/)

[![Avançando com Subqueries](https://www.devmedia.com.br/arquivos/cursos/curso_avancando-com-subqueries_2231.png)Avançando com SubqueriesCurso](https://www.devmedia.com.br/curso/curso-subqueries/2231)

[![Avançando no comando SQL Select](https://arquivo.devmedia.com.br/cursos/imagem/curso_consultas-basicas-em-sql_1902.jpg)Avançando no comando SQL SelectCurso](https://www.devmedia.com.br/curso/comando-select-sql/1902)

Tecnologias:

- [SQL](https://www.devmedia.com.br/sql/)