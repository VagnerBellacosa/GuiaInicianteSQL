# SQL: Cláusula Where

## Nesta documentação você aprenderá a utilizar o comando WHERE para adicionar filtros às suas consultas SQL.



<iframe src="https://www.youtube.com/embed/kYpG6tmseKY?rel=0" frameborder="0" allowfullscreen="" style="outline: none; -webkit-tap-highlight-color: transparent; max-width: 100%; margin: auto; height: 455.062px; width: 809px;"></iframe>

Uma tabela pode conter milhões de registros. Contudo, mover um volume tão grande de dados para a memória pode ser inviável. Além disso, na maioria dos casos precisamos modificar ou acessar poucos registros. A linguagem **SQL** nos permite parametrizar consultas para filtrar seu retorno.

### Cláusulas WHERE no SQL

Neste documento falaremos sobre como isso é possível através da cláusula **WHERE**.

#### Tópicos

[Sintaxe](https://www.devmedia.com.br/sql-clausula-where/37645#sintaxe)

[Tabelas de exemplo](https://www.devmedia.com.br/sql-clausula-where/37645#tabelas)

[Expressões lógicas](https://www.devmedia.com.br/sql-clausula-where/37645#expressoes)

[Igualdade](https://www.devmedia.com.br/sql-clausula-where/37645#igualdade)

[Diferença](https://www.devmedia.com.br/sql-clausula-where/37645#diferenca)

[Menor que](https://www.devmedia.com.br/sql-clausula-where/37645#menor)

[Maior que](https://www.devmedia.com.br/sql-clausula-where/37645#maior)

[BETWEEN](https://www.devmedia.com.br/sql-clausula-where/37645#between)

[LIKE](https://www.devmedia.com.br/sql-clausula-where/37645#like)

[IN](https://www.devmedia.com.br/sql-clausula-where/37645#in)

[AND e OR](https://www.devmedia.com.br/sql-clausula-where/37645#andor)

[Exemplo prático](https://www.devmedia.com.br/sql-clausula-where/37645#exemplo)

### Sintaxe

Usamos **WHERE** para filtrar dados em um comando **SQL**. Essa cláusula deve ser seguida por uma expressão lógica.

Vemos a seguir um exemplo da sintaxe dessa cláusula.

```
WHERE [condição(ões)]
```

### Tabela de exemplo

Para acompanhar os exemplos apresentados nessa documentação considere a seguinte tabela:

![Tabela Produtos](https://arquivo.devmedia.com.br/artigos/Estevao_Dias/DocumentacaoSQL/WHERE/tabela_produtos.png)**Figura 1**. Tabela Produtos

### Expressões lógicas

Chamamos de expressão lógica o conjunto de uma ou mais condições a serem verificadas pela cláusula **WHERE**. Essas expressões são construídas utilizando operadores, sobre os quais falaremos a partir desta seção.

### Igualdade

Este operador realiza a operação de igualdade entre dois valores. Com ele verificamos se o valor à direita do sinal = é igual ao valor contido na coluna informada à esquerda.

No exemplo a seguir buscamos um produto pela sua descrição:

```
SELECT
  descricao,
  precocusto
FROM
  produtos
WHERE
  descricao = "Caderno Escolar"
```

Obteremos como retorno a primeira linha na tabela de produtos.

### Diferença

Podemos ainda selecionar dados em uma tabela utilizando o operador <>, que aplica a operação de desigualdade entre dois valores.

No exemplo a seguir buscamos todos os produtos com categoria diferente de 2.

```
SELECT
  descricao,
  precocusto
FROM
  produtos
WHERE
  categoria <> 2
```

Ao final dessa consulta teremos apenas o último registro da tabela de produtos retornado, pois ele é o único que atende ao critério estabelecido.

### Menor que

O operador < verifica se o valor informado à esquerda é menor que àquele a direita. Podemos usar esse operador em conjunto com =, formando um único operador <=, chamado menor ou igual, para verificar se o valor é menor ou igual ao esperado.

No exemplo a seguir buscamos todos os produtos com preço de venda menor ou igual a 10:

```
SELECT
  descricao,
  precocusto
FROM
  produtos
WHERE
  precovenda <= 10
```

Visto que a tabela produtos possui dois registros com preço de venda menor ou igual a 10, apenas “Caneta Azul” será retornado.

### Maior que

O operador > verifica se o valor informado à esquerda é maior que àquele a direita. Podemos ainda combinar > e = resultando em >=, que verifica se o valor é maior ou igual.

No exemplo a seguir buscamos os produtos com preço de venda maior ou igual a 40:

```
SELECT
  descricao,
  precocusto
FROM
  produtos
  WHERE
  precovenda >= 40
```

Uma vez que a tabela produtos possui um registro com preço de venda maior ou igual a 40, “Carregador Portátil”, apenas esse registro será retornado.

### BETWEEN

Algumas vezes precisamos buscar registros de acordo com um intervalo de valores. Para isso contamos com o operador BETWEEN, que recebe um valor mínimo e um valor máximo e retorna os dados da coluna que atendem a esse critério.

No exemplo abaixo buscamos na tabela produtos os registros com preço de custo entre 1 e 5.

```
SELECT
  descricao,
  precocusto
FROM
  produtos
WHERE
  precovenda BETWEEN 1 AND 5
```

Para essa consulta será retornado apenas o produto “Caneta Azul”.

**Nota:** Podemos usar o operador NOT antes do BETWEEN - na forma NOT BETWEEN - para retornar os registros que não atendam à condição estabelecida. Executando a consulta acima com NOT BETWEEN teríamos o comportamento inverso retornando todos os registros, exceto “Caneta Azul”.

### LIKE

O operador LIKE permite comparar o valor de uma coluna com uma sequências de caracteres. Com ele podemos verificar se o texto na coluna contém uma palavra ou parte dela ou até mesmo uma sentença completa.

A sintaxe desse operador pode ser vista abaixo:



```
LIKE [sequência de caracteres]
```



Esse operador permite o uso do sinal %, que substitui um ou mais caracteres no início, meio ou final do texto utilizado como parâmetro. Exemplificando esse comportamento a seguir, usamos parte de uma palavra, “%ade%”, para buscar na coluna descrição por um texto que contenha “ade”, seja em seu início, meio ou final.

```
SELECT
  descricao,
  precocusto
FROM
  produtos
WHERE
  descricao LIKE "%ade%"
```

Uma vez que apenas “C**ade**rno” contém os caracteres “ade”, esse registro será retornado.

**Nota:** Podemos usar o operador NOT antes do LIKE - na forma NOT LIKE - para retornar os registros que não atendam à condição estabelecida. Executando a consulta acima com NOT IN todos os demais produtos seriam retornados, exceto “C**ade**rno”.

### IN

Com o operador INpodemos criar uma lista de valores que serão comparados com o valor na coluna. Dessa forma a linha a qual o valor avaliado pertence será ignorada exceto se qualquer um dos valores na lista corresponder ao valor da coluna.

Esta lista de valores pode conter tanto números quanto sequências de caracteres:

```
IN (1, 2, 3)
IN (“caderno”, “caneta”)
```

A seguir temos um exemplo do uso deste operador, no qual buscamos os produtos de código 1 e 3:

```
SELECT
  codigo,
  descricao
FROM
  produtos
WHERE
  codigo IN (1, 3)
```

Teremos como resultado dessa consulta as linhas 1 e 3 da tabela produtos.

**Nota:** Podemos usar o operador NOT antes do IN - na forma NOT IN - para retornar os registros que não atendam à condição fornecida. Aplicando NOT IN ao exemplo acima, apenas “Caneta” seria retornado.

### AND e OR

Há casos nos quais precisamos unir duas ou mais condições. Na linguagem **SQL** podemos realizar essa tarefa através de operadores lógicos como AND, que aplica a operação lógica E, e OR, que aplica a operação lógica OU.

A seguir listamos todos os produtos com preço de venda 40% maior que o preço de custo e que pertencem a categoria 2.

```
SELECT
  codigo,
  descricao,
  precocusto,
  precovenda
FROM
  produtos
WHERE
  precovenda >= (precocusto + (precocusto * 0.4)) AND categoria = 2
```

Ao final da consulta apenas a linha 2 da tabela será retornada.

**Nota:** O uso dos parênteses evita erros de lógica, pois por padrão as expressões dentro deles são avaliadas e resolvidas primeiro. Além disso, é uma boa prática organizar o código dessa forma para facilitar sua compreensão.

### Exemplo prático

No exemplo abaixo buscamos os produtos de categoria 2 ou que tenham preço de venda maior que 10 e que possuam preço de custo menor que 20.

```
SELECT
  codigo,
  descricao
FROM
  produtos
WHERE
  (categoria = 2 OR precovenda > 10) AND precocusto < 20
```

Como resultado obteremos as linhas 1 e 2, que atendem a todos os critérios estabelecidos.

#### Confira também

[![Cursos de SQL e Banco de Dados](https://arquivo.devmedia.com.br/cursos/imagem/curso_consultas-basicas-em-sql_1902.jpg)Cursos de SQL e Banco de DadosCurso](https://www.devmedia.com.br/cursos/banco-de-dados)

[![Curso de SQL](https://arquivo.devmedia.com.br/cursos/imagem/curso_1895.jpg)Curso de SQLCurso](https://www.devmedia.com.br/curso/curso-de-sql/1895)

[![Aprenda SQL](https://arquivo.devmedia.com.br/cursos/imagem/curso_409.jpg)Aprenda SQLGuia](https://www.devmedia.com.br/sql/)

