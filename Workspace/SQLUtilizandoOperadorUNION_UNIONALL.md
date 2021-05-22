# SQL: Utilizando o Operador UNION e UNION ALL

## O operador UNION combina os resultados de duas ou mais queries em um único result set, retornando todas as linhas pertencentes a todas as queries envolvidas na execução.

[Artigos](https://www.devmedia.com.br/artigos/)[Banco de Dados](https://www.devmedia.com.br/artigos/banco-de-dados)SQL: Utilizando o Operador UNION e UNION ALL

O **operador UNION** combina os resultados de duas ou mais queries em um **único result set**, retornando todas as linhas pertencentes a todas as queries envolvidas na execução. Para utilizar o UNION, o número e a ordem das colunas precisam ser idênticos em todas as queries e os data types precisam ser compatíveis.

Existem dois tipos de operador UNION, sendo eles UNION e UNION ALL.

### **UNION**

O [operador UNION](http://www.devmedia.com.br/produto-cartesiano-e-union-no-oracle/24590), por default, executa o equivalente a um SELECT DISTINCT no result set final. Em outras palavras, ele combina o resultado de execução das duas queries e então executa um SELECT DISTINCT a fim de eliminar as linhas duplicadas. Este processo é executado mesmo que não hajam registros duplicados.

Exemplo:

```
SELECT ShipName, ShipAddress from Orders WHERE CustomerID ="WARTH" UNION SELECT ShipName, ShipAddress from Orders WHERE CustomerID ="VINET"
```

**Resultado:**

```
ShipName                                        ShipAddress
---------------------------------------- -------------------
Vins et alcools Chevalier                     59 rue de l"Abbaye
Wartian Herkku                                  Torikatu 38
```



### **UNION ALL**

O operador **UNION ALL** tem a mesma funcionalidade do UNION, porém, não executa o *SELECT DISTINCT* no result set final e apresenta todas as linhas, inclusive as linhas duplicadas.

Exemplo:

```
SELECT ShipName, ShipAddress from Orders WHERE CustomerID ="WARTH"
```

**UNION ALL**

```
SELECT ShipName, ShipAddress from Orders WHERE CustomerID ="VINET"
```

**Resultado:**

```
ShipName                                 ShipAddress

-------------------------------- ------------------

Wartian Herkku                           Torikatu 38

Wartian Herkku                           Torikatu 38

Wartian Herkku                           Torikatu 38

Vins et alcools Chevalier              59 rue de l"Abbaye

Vins et alcools Chevalier              59 rue de l"Abbaye

Vins et alcools Chevalier              59 rue de l"Abbaye

Vins et alcools Chevalier              59 rue de l"Abbaye

Vins et alcools Chevalier              59 rue de l"Abbaye
```



### **RECOMENDAÇÕES**

\1) Se não existe a possibilidade de haver registros duplicados em suas tabelas ou se não houver problemas para a aplicação que o record set final apresente duplicações, utilize o operador **UNION ALL**. A vantagem é que este operador não executa a função **SELECT DISTINCT**, utiliza menos recursos do SQL Server e como consequência, melhora a performance da aplicação.

\2) Não utilize o operador **UNION** em conjunto com a função **SELECT DISTINCT** pois o resultado final será exatamente o mesmo, porém, o SQL Server estará executando a mesma operação duas vezes, causando queda de desempenho.

```
SELECT ShipName, ShipAddress from Orders WHERE CustomerID ="WARTH"
UNION
SELECT DISTINCT ShipName, ShipAddress from Orders WHERE CustomerID ="VINET"
```

**Resultado:**

```
ShipName                                 ShipAddress
---------------------------------------- -------------------
Vins et alcools Chevalier                59 rue de l"Abbaye
Wartian Herkku                           Torikatu 38
```

\3) Uma query com uma ou mais cláusula **OR** pode ser reescrita utilizando o operador **UNION ALL**:

```
SELECT employeeID, firstname, lastname
FROM names
WHERE dept = "prod" or city = "Orlando" or division = "food"
```

A consulta acima possui três condições separadas na cláusula WHERE. Sendo assim, para que esta query utilize um indice, as três colunas referenciadas (dept, city e division) deverão fazer parte do índice. Esta query pode ser reescrita utilizando-se o operador UNION ALL:



```
SELECT employeeID, firstname, lastname FROM names WHERE dept = "prod"
UNION ALLSELECT employeeID, firstname, lastname FROM names WHERE city = "Orlando"
UNION ALL
SELECT employeeID, firstname, lastname FROM names WHERE division = "food"
```



As duas consultas irão produzir o mesmo resultado. No entanto, se houver um índice na coluna dept mas não houver nas outras colunas referenciadas na cláusula WHERE, a primeira versão não utilizará o índice e será feito um *Table Scan* na tabela. Na segunda versão, o índice será utilizado em parte da query, melhorando significativamente resultado final.

Para mais informações sobre UNION ou UNION ALL, consulte o Books Online do SQL Server 2000.

![link](https://www.devmedia.com.br/layout/icon/link-button.png) Curso relacionado: [Administração de Banco de Dados SQL Server](http://www.devmedia.com.br/curso/curso-de-administracao-de-banco-de-dados-com-sql-server/406)