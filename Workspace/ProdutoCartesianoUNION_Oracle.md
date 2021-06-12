# Produto cartesiano e UNION no Oracle

## Veja nesse artigo o conceito de produto cartesiano e como utilizar o operador UNION no banco de dados Oracle.

[Artigos](https://www.devmedia.com.br/artigos/)[Banco de Dados](https://www.devmedia.com.br/artigos/banco-de-dados)Produto cartesiano e UNION no Oracle

No post de hoje vou explicar o conceito de produto cartesiano, o operador union e, no final do post, vamos fazer uma brincadeirinha para exemplificar os conceitos.

Vamos ao produto cartesiano. Quando precisamos selecionar dados de mais de uma tabela, temos que informar os campos (chaves) pelos quais as tabelas são ligadas, é a tal da "join condition". Ela pode ser escrita de duas formas: na clausula where, ou na clausula from. Vejamos os exemplos a seguir:

**Listagem 1:**Exemplo Join com condição na cláusula FROM

```
SELECT tabela1.campo1, tabela1.campo2, tabela2.campo1
FROM tabela1 INNER JOIN tabela2 ON tabela1.campoPK = tabela2.campoFK;
```

**Listagem 2:** Exemplo de join com a condição na clausula WHERE:

```
SELECT tabela1.campo1, tabela1.campo2, tabela2.campo1
FROM tabela1, tabela2
WHERE tabela1.campoPK = tabela2.campoFK;
```

Quando, por algum motivo(geralmente esquecimento...), omitimos a join condition o resultado da nossa consulta é um produto cartesiano que é o relacionamento de cada linha da primeira tabela com cada linha da segunda tabela(no caso de haver somente duas tabelas). O numero de registros retornados é a multiplicação dos numeros de registros de cada tabela. Exemplo:

- Tabela1, 1000 registros;
- Tabela2, 2000 registros;
- Numero de registros retornados no produto cartesiano entra a tabela1 e a tabela2: 1000 x 2000 = 2.000.000 de registros!!!

Regra: Numero de registros no produto cartesiano = count(tabela1) x count(tabela2) x ... x count(tabelaN).

*NÃO TESTE UM PRODUTO CARTESIANO EM UM AMBIENTE DE PRODUÇÃO!!! INSTALE O ORACLE NA SUA MAQUINA PARA FAZER O TESTE.*

Apesar de o produto cartesiano, na maioria das vezes, não retornar alguma informação útil, vou usá-lo aqui na nossa brincadeira.

Agora o operador UNION. Representa a união(U) da teoria dos conjuntos que aprendemos lá no colegial(colegial??? to ficando velho...). Podemos usá-lo para unir duas consultas em um só resultado. Geralmente fazemos isso quando não é possível escrever toda a condição na clausula where ou quando usando UNION nossa consulta fique mais clara. Exemplo: Preciso selecionar o nome de todos os funcionários ativos e os demitidos entre 01/03/2011 e 31/03/2011.

**Listagem 3:** Sem UNION

```
SELECT nome
FROM funcionario
WHERE (FLG_DEMITIDO = 'N') OR (FLG_DEMITIDO = 'S' AND TRUNC(DATA_DEMISSAO) BETWEEN '01-mar-2011' and '31-mar-2011');
```

**Listagem 4:** Com UNION:

```
--Funcionários ativos
SELECT nome
FROM funcionario
WHERE (FLG_DEMITIDO = 'N')
UNION
--Funcionários demitidos
SELECT nome
FROM funcionario
WHERE FLG_DEMITIDO = 'S'
AND TRUNC(DATA_DEMISSAO) BETWEEN '01-mar-2011' and '31-mar-2011';
```

No meu ponto de vista, a consulta usando UNION fica bem mais clara e fácil de alterar se preciso. Porém, para usar o union precisamos seguir algumas regras:

1. Todas as consultas devem ter o mesmo numero de colunas selecionadas. No exemplo a primeira consulta seleciona o nome do funcionário, logo a segunda poderá selecionar apenas o nome também.
2. Os tipos de dados das colunas selecionadas devem ser os mesmos na ordem em que aparecerem. Se estivéssemos selecionando 2 colunas: Nome(varchar2) e data_nascimento(date) a segunda consulta deveria selecionar as colunas na mesma ordem, para que os tipos de dados correspondam.

Só mais coisa... o operador UNION exclui os registros duplicados. Se você precisar deles troque UNION por UNION ALL.

Também precisamos esclarecer uma coisa sobre a clausula FROM. Ela lista o “conjunto de dados” sobre o qual queremos selecionar alguma coisa. O conjunto de dados pode ser uma tabela, uma view ou até mesmo uma consulta. Na demonstração usarei duas consultas na clausula FROM.

Antes da nossa brincadeira só precisamos saber mais uma coisa: o que é a tabela DUAL do Oracle. É uma tabela do dicionário de dados que possui uma linha e uma coluna freqüentemente usada para selecionar alguma coisa que não está em tabela nenhuma. Exemplo: SELECT sysdate FROM DUAL; retorna a data do banco de dados.

Chega de conversa, é hora da diversão! Vamos fazer uma tabuada com uma instrução SQL.

Antes de escrever qualquer código pense em como a tabuada é construída. Se precisássemos fazê-la em alguma linguagem de programação como faríamos? Em C, por exemplo, poderíamos usar dois laços for:

**Listagem 5:** Código de tabuada em linguagem C

```
for(int i = 1; i <= 10; i++)
{
	for(int j = 1; j <= 10; j++)
	{
		printf(“%d X %d = %d \n”, i, j, i * j);
}
}
```

A lógica acima resolve o nosso problema. O problema é que não temos um laço for na linguagem SQL (ele existe no [PL/SQL do Oracle](http://www.devmedia.com.br/curso/pl-sql-oracle/148) e no T-SQL do SQL Server), mas não usaremos aqui. É aí que entra o nosso produto cartesiano. Se tivermos duas tabelas de uma coluna com 10 linhas e seus valores variando de 0 a 10 podemos fazer o produto cartesiano delas.

Mas também não temos essas tabelas. Vamos criá-las dinamicamente na clausula FROM usando o operador UNION.

Execute o seguinte comando no SQL Plus:

**Listagem 6:** Consulta simples

```
SELECT 1 as x FROM DUAL;
```

O resultado será 1. Agora execute este:

**Listagem 7:** Consulta com UNION

```
SELECT 1 as x FROM DUAL
UNION
SELECT 2 as x FROM DUAL;
```

Agora você terá 1 e 2 como resultado. Dessa forma podemos criar a tabela para usar na nossa tabuada. O código completo da tabuada fica assim:

**Listagem 8:** Código completo da tabuada com SQL

```
select a.x || ' X ' || b.y || ' = ' || a.x * b.y as TABUADAS
from (select 1 as x from dual
union
select 2 as x from dual
union
select 3 as x from dual
union
select 4 as x from dual
union
select 5 as x from dual
union
select 6 as x from dual
union
select 7 as x from dual
union
select 8 as x from dual
union
select 9 as x from dual
union
select 10 as x from dual) a,
(select 1 as y from dual
union
select 2 as y from dual
union
select 3 as y from dual
union
select 4 as y from dual
union
select 5 as y from dual
union
select 6 as y from dual
union
select 7 as y from dual
union
select 8 as y from dual
union
select 9 as y from dual
union
select 10 as y from dual) b
```

Vamos às explicações:

1. Na clausula FROM criamos duas tabelas com uma coluna e 10 linhas dinamicamente e as chamamos de “a” e “b”. A coluna da tabela “a” se chama “x” e da tabela “b” se chama “y”.
2. No select concatenamos a coluna “x” da tabela “a” com o “x” que é símbolo da multiplicação. Depois juntamos com a coluna “y” da tabela “b”, o sinal de igual (“=”) e a multiplicação de a.x * b. y. No final colocamos um alias de TABUADAS.

Vejam que foi bem simples, só precisamos saber usar dois conceitos da Álgebra Relacional: o Produto Cartesiano e o operador UNION.

Espero ter esclarecido estes conceitos, mas se não esclareci nada postem as dúvidas nos comentários.