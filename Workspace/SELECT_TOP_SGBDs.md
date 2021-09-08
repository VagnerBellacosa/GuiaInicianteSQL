# SELECT TOP em vários SGBDs

## Veja neste artigo como utilizar a cláusula TOP, como é mais conhecida, para limitar a quantidade de registros retornados em uma consulta. Serão apresentadas as sintaxes dessa instrução no SQL Server, MySQL, PostgreSQL, Oracle e Firebird.



A cláusula TOP da linguagem SQL é utilizada para limitar o número de registros retornados por uma consulta e é pode, por exemplo, garantir certo ganho de desempenho em algumas consultas que normalmente seriam compostas por uma quantidade muito grande de registros.

São muitos os usos possíveis dessa instrução, então vou citar apenas uma, a que utilizarei para os exemplos desse artigo.

Suponha que em uma tabela do banco de dados sejam armazenados os dados dos jogadores de um determinado jogo, tais como código, nome e pontuação. Em determinado momento, deseja-se obter um ranking com os três primeiros ou melhores jogadores, tomando como critério a pontuação. Para isso, é possível utilizar a cláusula TOP, como veremos a seguir.

Neste artigo, explicarei como utilizar o TOP em diferentes [SGBD’s: SQL Server, MySQL, PostgreSQL, Oracle e Firebird](https://www.devmedia.com.br/os-nove-passos-do-banco-de-dados/9567). Então, inicialmente precisamos conhecer a sintaxe básica desse comando.

**Listagem 1:** Sintaxe de uso da cláusula TOP no SQL Server

```
SELECT TOP [NUMERO DE REGISTROS] [COLUNA(S)] FROM [TABELA]
```

**Listagem 2:** Sintaxe de uso da cláusula TOP no MySQL e PostgreSQL

```
SELECT [COLUNA(S)] FROM [TABELA] LIMIT [NUMERO DE REGISTROS]
```

**Listagem 3:** Sintaxe de uso da cláusula TOP no Oracle

```
SELECT [COLUNA(S)] FROM [TABELA] WHERE ROWNUM < [NUMERO DE REGISTROS]
```

**Listagem 4:** Sintaxe de uso da cláusula TOP no Firebird

```
SELECT FIRST [NUMERO DE REGISTROS] [COLUNA(S)] FROM [TABELA]
```

O parâmetro **[NUMERO DE REGISTROS]** indica a quantidade de linhas que a consulta deve retornar. No caso do ranking dos três primeiros jogadores, esse valor seria 3. A partir desse ponto, a consulta mantém sua sintaxe padrão.

Vejamos na prática essa consulta funcionando. No caso, estarei utilizando apenas o SQL Server para apresentar os resultados da utilização desta cláusula. Inicialmente inseri alguns dados fictícios na tabela, como se pode ver na figura a seguir.

![Dados iniciais na tabela JOGADORES](https://arquivo.devmedia.com.br/artigos/Joel_Rodrigues/SELECT_TOP/SELECT_TOP1.png)

**Figura 1:** Dados iniciais na tabela JOGADORES

Para ilustrar o exemplo do ranking, selecionarei os três jogadores mais bem colocados e, em seguida, os três últimos colocados.

**Listagem 5:** Selecionando os três primeiros jogadores no SQL Server

```
SELECT TOP 3 * FROM JOGADORES ORDER BY PONTOS DESC
```

O resultado é exibido na **Figura 2** a seguir.

![Três primeiros jogadores no ranking](https://arquivo.devmedia.com.br/artigos/Joel_Rodrigues/SELECT_TOP/SELECT_TOP2.png)

**Figura 2:** Três primeiros jogadores no ranking

Vejamos agora os três últimos colocados.

**Listagem 6:** Três últimos jogadores no SQL Server

```
SELECT TOP 3 * FROM JOGADORES ORDER BY PONTOS
```

![Três últimos jogadores no ranking](https://arquivo.devmedia.com.br/artigos/Joel_Rodrigues/SELECT_TOP/SELECT_TOP33.png)

**Figura 3:** Três últimos jogadores no ranking

A seguir, veremos as instruções equivalentes nos demais SGBD’s citados.

**Listagem 7:** Três primeiros jogadores no MySQL e PostgreSQL

```
SELECT * FROM JOGADORES ORDER BY PONTOS DESC LIMIT 3
```

**Listagem 8:** Três primeiros jogadores no Oracle

```
SELECT * FROM JOGADORES ORDER BY PONTOS DESC WHERE ROWNUM < 3
```

**Listagem 9:** Três primeiros jogadores no Firebird

```
SELECT FIRST 3 * FROM JOGADORES ORDER BY PONTOS DESC
```

**Listagem 10:** Três últmos jogadores no MySQL e PostgreSQL

```
SELECT * FROM JOGADORES ORDER BY PONTOS LIMIT 3
```

**Listagem 11:** Três últimos jogadores no Oracle

```
SELECT * FROM JOGADORES ORDER BY PONTOS WHERE ROWNUM < 3
```

**Listagem 12:** Três últimos jogadores no Firebird

```
SELECT FIRST 3 * FROM JOGADORES ORDER BY PONTOS
```

Complementando a Listagem 12, poderíamos querer ordenar a listagem dos três últimos em ordem crescente, para isso bastaria fazer como mostra o código a seguir.

**Listagem 13:** Reordenando a consulta no Firebird

```
SELECT * FROM
      (SELECT FIRST 3 * FROM JOGADORES ORDER BY PONTOS DESC)
      ORDER BY PONTOS
```

Ou seja, foi feita uma consulta externa à principal para reordenar o resultado original.

### Conclusão

O uso da cláusula TOP (como é mais conhecida) pode garantir ganho de performance para a aplicação e é, na maioria das vezes, mais viável que fazer essa limitação de registros por código, seja qual for a linguagem utilizada.

Espero que este breve artigo possa ajudar àqueles que utilizam esses SGBD's.

Um abraço e até a próxima.

Tecnologias:

- [Banco de Dados](https://www.devmedia.com.br/banco-de-dados/)