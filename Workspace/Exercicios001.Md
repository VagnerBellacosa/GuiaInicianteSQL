## DDL, DML e DQL‎ > ‎

### Exercícios
Tabela CLIENTE

Codigo            Nome              SobreNome             Data             Idade               CPF                Sexo

Integer(PK)     Varchar(20)    Varchar(30)                  date              Integer             Integer             Varchar(10)

Not Null          Not Null          Null                              Not Null         Not Null           Not Null            Not Null


1 - Crie a tabela acima usando o comando em SQL CREATE TABLE, com as suas devidas estruturas.

2 - Conforme a tabela Cliente, adicione um campo Salario (FloatNotnul), usando o comando em SQL ALTER TABLE...ADD

3 - Com o comando em SQL DROP, exclua a tabela Cliente com suas estruturas. DROP TABLE.

4 - Com o comando em SQL CHANGE, troque o nome do campo Data para Data_NASC. 

5 - De acordo com a tabela Cliente, insira 4 tuplas usando o comando em SQL  INSERT INTO, conforme dados abaixo:

| Codigo |  Nome  |   SobreNome  |  Data_Nasc  |    Idade  |   CPF  |        Sexo |   Salario |
| --- |  --- |  --- |  --- |  --- |  --- |  --- |  --- | 
| 01  |      Leo     |         Pires  |      10/10/1980 |     34 |        123456789  |     M  |           540,00 |
| 02  |      Leoncio |         Silva  |      01/05/1982 |     32 |        987654321  |     M  |         1.500,00 |
| 03  |      Leandro |         Souza  |      30/12/1960 |     54 |        654987321  |     M  |         2.000,00 |
| 04  |      Ana     |         Felix  |      25/11/1962 |     52 |        321456789  |     F  |         2.300,00 |


6 - Usando os comandos em SQL UPDATE e WHERE, Altere o valor do campo Salário para 1.300,00, do código 04.

7 - Apague o registro 04, utilizando os comando em SQL DELETE e WHERE.

8 - Exiba a estrutura da tabela Cliente usando o comando em SQL DESCRIBE. 

9 - Selecione toda a Tabela Cliente, usando o comando SELECT.

10- Selecione somente os campos código, nome e idade da tabela Cliente, usando o comando SELECT.

11- Selecione o Cliente Leo e exiba o seu salário e idade, usando os comandos SELECT e WHERE

12- Selecione todos os campos da tabela Cliente que contenham salários maiores que R$1.000 e ordene de forma decrescente o salário, usando os comandos em SQL Select, Where e order by

13- Na tabela Cliente, adicione um campo Bairro (Char20 not null).

14- Adicione as informações no campo Bairro, conforme tabela abaixo:

Código           Bairro

1                     Centro
2                     Centro
3                     Icaraí

15- Na tabela Cliente, Adicione uma tupla conforme dados abaixo:
Codigo      Nome          SobreNome      Data_Nasc    Idade      CPF              Sexo       Salário           Bairro

04              Paula           Castro                 30/11/19630     51       221456789       F             680,00           Ingá



16- Selecione somente os Bairros, usando o comando DISTINCT.

17- Selecione todos os nomes, sobrenomes e CPF, onde o campo Nome comece por Leo, usando o comando em SQL LIKE.

18- Exiba o maior salário da tabela Cliente, usando o comando em SQL MAX.

19- Exiba a menor idade por bairro da tabela Cliente, usando o comando em SQL MIN, GROUP BY 

20- Exiba as quantidades de Tuplas que contenham nomes iniciados com “Le”.