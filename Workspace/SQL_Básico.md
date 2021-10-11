# SQL Básico

## Veja nesse artigo alguns comandos básicos de SQL, a linguagem mais utilizada em banco de dados.



A **linguagem SQL** é a mais utilizada quando o assunto é manipulação de banco de dados, e nesse artigo vamos aprender a trabalhar com essa linguagem e ver alguns exemplos.

### Comando “select”

O comando select é o “comando base” para todos os comandos de pesquisa de informações no banco de dados.

```
select * from pcnfsaid;
```

**Listagem 1**. Selecionando todas as colunas

O código da **Listagem 1** seleciona todos os registros da tabela pcnfsaid, sem restrição de campos.

```
select numnota, dtsaida, vltotal from pcnfsaid;
```

**Listagem 2**. Selecionando colunas específicas

O código da **Listagem 2** seleciona os campos numnota, dtsaida e vltotal de todos os registros da tabela pcnfsaid.

### Usando operadores aritméticos (usando ou não parênteses)

O código da **Listagem 3** seleciona os campos numnota, dtsaida, vltotal, vldesconto, calcula vltotal-vldesconto, colhendo informações de todos os registros da tabela pcnfsaid.

- Regras de precedência: multiplicação(*), divisão(/), adição(+), subtração(-).
- Multiplicação e divisão têm prioridade sobre adição e subtração.
- Operadores de mesma prioridade são executados da esquerda para a direita.
- Parênteses podem ser usados para forçar a prioridade dos cálculos e tornar o entendimento das fórmulas mais claro.

```
select numnota, dtsaida, vltotal, vldesconto, vltotal-vldesconto
from pcnfsaid;
```

**Listagem 3**. Como usar operadores aritméticos

### Definindo apelido para campos selecionados

A atribuição de apelidos aos campos selecionados tem como objetivo facilitar o entendimento das informações exibidas nos resultados das pesquisas.

Existem quatro maneiras diferentes de atribuir apelidos aos campos selecionados, são elas:

- select codprod as codigo from pcprodut;
- select codprod codigo from pcprodut;
- select codprod as “Codigo do Produto” from pcprodut;
- select codprod “Codigo do Produto” from pcprodut;

Observe que a única diferença entre os exemplos 1 e 2 é a utilização ou não do termo as. A utilização desse termo é opcional, e tem como objetivo melhorar o entendimento das seleções efetuadas. Esta diferença também pode ser observada entre os exemplos 3 e 4.

Observe que enquanto nos exemplos 1 e 2 os apelidos são escritos sem a utilização de aspas(“), os exemplos 3 e 4 fizeram a utilização desse caractere. A utilização de aspas(“) é necessária quando o apelido a ser atribuído contém espaços em branco.

### Concatenar

O comando concatenar consiste em unificar as informações de dois campos distintos da tabela utilizada, exibindo-as em apenas um campo. Também podem ser utilizadas strings que não sejam informações existentes nas tabelas.

A **Listagem 4** seleciona o campo especie, concatenando com o campo seria, seleciona também o campo numnota, de todos os registros da tabela pcnfsaid.

```
select especie || serie, numnota from pcnfsaid;
```

**Listagem 4**. Exemplo de concatenação

Já a **Listagem 5** mostra como concatenar a string Emitido em: com o valor do campo dtemissao, com a string com vencimento em:, com o valor do campo dtvenc, onde os campos são da tabela pcprest, utilizando todos os registros da tabela.

```
select “Emitido em:” || “dtemissao:” || “ com vencimento
em:” || dtvenc from pcprest;
```

**Listagem 5**. Outro exemplo de concatenação

### Distinct

O comando distinct consiste em exibir as informações da tabela utilizada de maneira distinta, assim as informações não serão exibidas de maneira redundante.

```
select distinct codcli from pcnfsaid;
```

**Listagem 6**. Usando Distinct

O código da **Listagem 6** mostra como selecionar o campo codcli da tabela pcnfsaid de maneira distinta, ou seja, cada informação do campo codcli será exibida apenas uma vez, não importando quantas outras vezes essa informação esteja presente em outros registros da tabela utilizada.

### Restringindo Dados (where)

O comando de restrição where é o mais utilizado. Ele quer dizer “onde”, ou seja, com a sua utilização serão exibidas as informações solicitadas, obedecendo às restrições informadas.

Documentação: [SQL: Cláusula Where](http://www.devmedia.com.br/sql-clausula-where/37645)

### Condições de Comparação (=, >, >=, <, <=, <>, in, like, between, is null)

#### “=“ é igual

A **Listagem 7** seleciona todos os campos dos registros da tabela pcnfsaid onde o valor do campo numnota for igual a 50.

```
select * from pcnfsaid where  numnota = 50;
```

**Listagem 7**. Usando where com =

#### “>” é maior

A **Listagem 8** seleciona todos os campos dos registros da tabela pcnfsaid onde o valor do campo numnota for maior que 45.

```
select * from pcnfsaid where numnota > 45;
```

**Listagem 8**. Usando where com >

#### “>=“ é maior ou igual

O código da **Listagem 9** seleciona todos os campos dos registros da tabela pcnfsaid onde o valor do campo numnota for maior ou igual a 50.

```
select * from pcnfsaid where  numnota >=50;
```

**Listagem 9**. Usando where com >

#### “<” é menor

O código da **Listagem 10** seleciona todos os campos dos registros da tabela pcnfsaid onde o valor do campo numnota for menor que 10.

```
select * from pcnfsaid where  numnota < 10;
```

**Listagem 10**. Usando Where com <

#### “<=“ é menor ou igual

Na **Listagem 11** selecionamos todos os campos dos registros da tabela pcnfsaid onde o valor do campo numnota for menor ou igual a 5.

```
select * from pcnfsaid where numnota <= 5;
```

**Listagem 11**. Usando where com <=

#### “<>” é diferente

O código da **Listagem 12** seleciona todos os campos dos registros da tabela pcnfsaid onde o valor do campo numnota for diferente de 2.

```
select * from pcnfsaid where numnota <> 2;
```

**Listagem 12**. Usando where com diferente(<>)

#### “in” é na lista

Na **Listagem 13** selecionamos todos os campos dos registros da tabela pcnfsaid onde o valor do campo numnota pertencer à lista 1,3,5,6,8.

```
select * from pcnfsaid where numnota in (1,3,5,6,8);
```

**Listagem 13**. Usando where com In

#### “like” é como, parecido

Selecionamos todos os campos dos registros da tabela pcnfsaid na **Listagem 14**, onde o valor do campo numnota começa com o número 1, não importando quantos e quais números venham na sequência.

```
select * from pcnfsaid where numnota like “1%”;
```

**Listagem 14**. Usando Like

Já a **Listagem 15** seleciona todos os campos dos registros da tabela pcnfsaid, onde o valor do campo numnota começa com o número 1, e que tenham apenas um número na sequência, com qualquer valor.

```
select * from pcnfsaid where numnota like “1_”;
```

**Listagem 15**. Outro exemplo de like

E na **Listagem 16** selecionamos todos os campos dos registros da tabela pcnfsaid, onde o valor do campo numnota começa com o número 1, e que tenham apenas dois números na sequência, com qualquer valor.

```
select * from pcnfsaid where numnota like “1__”;
```

**Listagem 16**. Mais um exemplo de like

Selecionamos todos os campos dos registros da tabela pcnfsaid, onde o primeiro número do campo numnota tenha qualquer valor, o segundo número for 1, não importando quantos e quais números venham na sequência, conforme mostra a **Listagem 17**.

```
select * from pcnfsaid where numnota like “_1%”;
```

**Listagem 17**. Outra opção de uso do like

### “between” é a cláusula que coloca a busca entre dois valores

O código da **Listagem 18** seleciona todos os campos dos registros da tabela pcnfsaid, onde o valor do campo numnota esteja entre os valores 1 e 10, incluindo-os.

```
select * from pcnfsaid where numnota between 1 and 10;
```

**Listagem 18**. Cláusula between

Na condição between os valores informados como limites são considerados nos resultados.

#### “is null” quer dizer é nulo

O código da **Listagem 19** seleciona todos os campos dos registros da tabela pcnfsaid, onde o valor do campo comissao for nulo.

```
select * from pcnfsaid where  comissao is null;
```

**Listagem 19**. Usando is null

Campo nulo é o campo que não possui nenhum valor atribuído. Campos com um caractere de espaço ou 0 (zero) tem valor atribuído. Campo nulo é campo vazio.

### Condições Lógicas (not, and, or)

#### NOT

A condição lógica not é utilizada para acrescentar uma condição de negação, possibilitando a exclusão de registros na seleção, por exemplo a **Listagem 20**.

```
select * from pcprest
where  duplic = 4
and    codcob not in (“DESD”,”CHP”);
```

**Listagem 20**. Usando NOT

O código acima seleciona todos os campos dos registros da tabela pcprest, onde o campo duplic for igual a 4, e o campo codcob não for DESD e CHP.

```
select * from pcnfsaid
where  comissao is not null;
```

**Listagem 21**. Usando is not null

O código da **Listagem 21** seleciona todos os campos dos registros da tabela pcnfsaid, onde o campo comissão não for nulo.

O código da **Listagem 22** seleciona todos os campos dos registros da tabela pcclient, onde o conteúdo do campo cliente não seja parecido com “JOSE”, ou seja, o nome “JOSE” não pode ser parte do conteúdo do campo cliente.

```
select * from pcclient
where cliente not like “%JOSE%”;
```

**Listagem 22**. Usando not like

O código da **Listagem 23** seleciona todos os campos dos registros da tabela pcclient, onde o valor do campo dtcadastro não estiver entre 01-jan-2007 e 31-jan-2007.

```
select * from pcclient
where dtcadastro not between “01-jan-2007” and “31-jan-2007”;
```

**Listagem 23**. Usando not between

#### AND

A condição lógica and é utilizada para acrescentar uma condição de inclusão, ou seja, o resultado será exibido apenas se todos os critérios de pesquisa forem obedecidos.

O código da **Listagem 24** seleciona todos os campos dos registros da tabela pcprest, onde o campo duplic for igual a 2 e o campo codcob for diferente de DESD.

```
select * from pcprest
where  duplic = 2
and    codcob <> “DESD”;
```

**Listagem 24**. Usando condição AND

#### OR

A condição lógica or é utilizada para acrescentar uma condição de alternativa, ou seja, o resultado será exibido caso seja obedecido o primeiro critério OU caso seja obedecido o segundo critério.

A **Listagem 25** seleciona todos os campos dos registros da tabela pcprest, onde o campo duplic for igual a 2, e o campo codcob for igual a BK, ou onde o campo duplic for igual a 3, e o campo codcob for igual a CHP.

```
select * from pcprest
where  duplic = 2
and    codcob = “BK”
or     duplic = 3
and    codcob = “CHP”;
```

**Listagem 25**. Usando OR

No exemplo acima citado, observe que serão obedecidas as condições antes do OR ou depois do “OR”, não importando quantas outras condições sejam utilizadas.

### Ordenando os resultados (order by, order by desc)

#### Ordenando por coluna simples

A **Listagem 26** seleciona todos os campos dos registros da tabela pcempr, ordenando as informações pelo campo matricula, em ordem crescente.

```
select * from pcempr
order  by matricula;
```

**Listagem 26**. Ordenação em coluna simples

O código da **Listagem 27** seleciona todos os campos dos registros da tabela pcempr, ordenando as informações pelo campo matricula, em ordem decrescente.

```
select * from pcempr
order  by matricula desc;
```

**Listagem 27**. Usando ordenação decrescente

#### Ordenando por múltiplas colunas

A **Listagem 28** seleciona todos os campos dos registros da tabela pcprest, ordenando as informações pelos campos duplic, codcli e codcob, em ordem crescente.

```
select * from pcprest
order  by duplic, codcli, codcob;
```

**Listagem 28**. Usando ordenação por múltiplas colunas

O código da **Listagem 29** seleciona todos os campos dos registros da tabela pcprest, ordenando as informações pelos campos duplic, em ordem crescente, codcli em ordem crescente e codcob em ordem decrescente.

```
select * from pcprest
order  by duplic, codcli, codcob desc;
```

**Listagem 29**. Ordenando em ordem decrescente

##### Conclusão

Nesse artigo vimos algumas operações básicas da **linguagem SQL**. Com a leitura dele qualquer pessoa agora pode começar a estudar e fazer diversos testes em bancos de dados.

Espero que tenham gostado e até o próximo artigo.