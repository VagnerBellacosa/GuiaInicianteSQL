# Escrevendo queries otimizadas no SQL Server

## O artigo descreve algumas das principais técnicas associadas à escrita de queries otimizadas, mostrando estruturas que devem ser evitadas dentro das consultas por as tornarem menos eficazes.



Por que eu devo ler este artigo:Além de ser um modelo de boas práticas a serem seguidas, estas técnicas de otimização de consultas ajudam a otimizar o tempo de resposta dos sistemas em ambientes com limitações de hardware e banda de rede.

[Voltar](https://www.devmedia.com.br/sql-server/sql-server-para-dbas)Suporte ao alunoAnotarMarcar como concluído

[Artigos](https://www.devmedia.com.br/artigos/)[Banco de Dados](https://www.devmedia.com.br/artigos/banco-de-dados)Escrevendo queries otimizadas no SQL Server

O artigo descreve algumas das principais técnicas associadas à **escrita de queries otimizadas**, mostrando estruturas que devem ser evitadas dentro das consultas por as tornarem menos eficazes.

O uso destas técnicas serve para extrairmos ao máximo a performance do banco de dados, fazendo o processador de consultas trabalhar a favor na hora de escolher o melhor plano de execução.

[Saiba maisSérie **SQL nível Jedi: Subqueries**](https://www.devmedia.com.br/sql-queries/)

Fazer o ajuste (tunning) de uma consulta é mais uma arte do que uma ciência. Uma mesma query pode ter um comportamento totalmente diferente mediante ao montante de registros existente, às chaves presentes, aos recursos do sistema, às estatísticas de dados, aos índices e outros fatores. Neste contexto, este artigo descreve algumas das principais técnicas associadas à **escrita de queries otimizadas**, mostrando estruturas que devem ser evitadas dentro das consultas por as tornarem menos eficazes.

Neste artigo vamos explorar as **técnicas de otimização de queries** para mobilizarmos o banco a trabalhar da forma mais otimizada possível.

Abordaremos algumas das técnicas mais avançadas existentes para reconstrução de cursores, expressões de pivotamento (PIVOT) e estratégias de otimização de índices, completando todo o estudo para manter um ambiente de banco de dados que preza pela performance de acordo com as boas práticas orientadas pela engenharia da equipe do [SQL Server](https://www.devmedia.com.br/guia/sql-server/35720).

### Diretrizes para construção de queries eficientes

Fazer o ajuste (*tunning*) de uma consulta é mais uma arte do que uma ciência. Uma mesma query pode ter um comportamento totalmente diferente mediante ao montante de registros existente, às chaves presentes, aos recursos do sistema, às estatísticas de dados, aos índices e outros fatores.

O melhor conselho é considerar cada consulta caso a caso e tentar uma variedade de técnicas de otimização. Somente assim você pode aprender com certeza como o seu sistema responde às diferentes técnicas de tunning.

Alguns cuidados precisam ser tomados ao escrever uma query, dentre eles podemos citar:

1. Favoreça a lógica de conjuntos ao invés da lógica de procedimentos;

2. 1. O fator mais importante a ser considerado quando **estiver otimizando queries** é saber como funciona a manipulação da lógica de conjuntos de registros;
   2. Cursores e outras construções procedimentais geralmente limitam a habilidade do otimizador de consultas para gerar planos de consultas flexíveis.

1. Teste variações de queries objetivando a performance;

2. 1. Frequentemente o otimizador pode produzir planos totalmente diferentes para queries logicamente equivalentes;
   2. Teste técnicas diferentes, como joins ou subqueries, para descobrir qual delas é mais eficiente.

1. Evite QUERY HINTS (dar dica ao banco sobre qual índice ele deve usar);

2. 1. Query hints diz ao otimizador como se comportar, portanto, sobrescreve a habilidade do otimizador de fazer o seu trabalho adequadamente. Eliminando as escolhas do otimizador, você irá se limitar a um plano que é menos eficaz que o ideal;
   2. Utilize essa técnica somente quando tiver absoluta certeza que o otimizador está incorreto na sua escolha perante a lógica do seu negócio.

1. Use subqueries correlatas;

2. 1. O otimizador está apto a trabalhar para integrar subqueries no fluxo da query principal em uma variedade de caminhos escolhidos por ele;
   2. As subconsultas podem ser úteis em diversas situações para o ganho de performance, por exemplo, ao realizar um JOIN para uma tabela apenas para verificar a existência de linhas correlatas;
   3. Para uma melhor performance, **troque estes tipos de JOINS por queries correlatas que fazem uso do operador EXISTS**, como mostrado na **Listagem 1**.

**Listagem 1.** Usando o LEFT JOIN e o NOT EXISTS.

```
SELECT   a.pk_tabela_pai
FROM     dbo.tabela_pai   a
LEFT JOIN dbo.tabela_filha b
ON       a.pk_tabela_pai   = b.fk_tabela_filha
WHERE    b.fk_tabela_filha IS NULL
GO
SELECT   a.pk_tabela_pai
FROM     dbo.tabela_pai a
WHERE NOT EXISTS(SELECT *
                FROM   dbo.tabela_filha b
WHERE  a.pk_tabela_pai
       b.fk_tabela_filha)
```

1. Evite funções definidas pelo usuário na cláusula WHERE;

2. 1. Funções definidas pelo usuário que retornam apenas um valor, diferentemente das subqueries escalares, não são otimizadas no plano principal da query;
   2. Usar função escalar na lista do SELECT é muito menos problemático porque as linhas já foram filtradas na cláusula WHERE.

1. Use funções tabulares (*table-valued*) como tabelas derivadas;

2. 1. São funções que retornam o resultado em formato de tabela;
   2. Ao contrário do item cinco, funções tabulares são geralmente úteis, no ponto de vista de performance, quando você as utiliza como tabelas derivadas;
   3. O otimizador avalia uma tabela derivada apenas uma vez por consulta. Se uma função definida pelo usuário em uma TABLE-VALUED tiver uma lógica que atenda outras consultas, você pode encapsular e reutilizá-la para outras queries.

1. Evite colunas GROUP BY desnecessárias;

2. 1. Quanto mais colunas na lista da cláusula GROUP BY você adicionar, mais o processo de agrupar linhas torna-se dispendioso;
   2. Se a sua consulta possui poucas colunas de agregação, mas muitas colunas agrupadas não agregadas, você deve pensar em reconstruí-la **utilizando subqueries escalares correlatas**. Isso resultará em menos trabalho para agrupar na consulta.

1. Use expressões CASE;

2. 1. A expressão CASE é uma das mais poderosas ferramentas de lógica disponível para os programadores T-SQL. Utilizando essa expressão você pode, por exemplo, mudar dinamicamente a saída de uma coluna;
   2. Isso habilita a consulta a retornar somente os dados que são absolutamente necessários, reduzindo assim operações de I/O e rede, ao invés de gerar um RESULT SET muito grande e desnecessário ao cliente.

1. Divida JOINS em tabelas temporárias.

2. 1. A estratégia principal do otimizador é encontrar planos que satisfaçam consultas utilizando operações simples;
   2. Embora essa estratégia funcione para a maioria dos casos, ela pode falhar quando a saída de dados for grande, porque muitos joins requerem muita operação de I/O;
   3. Em alguns casos, a melhor opção é reduzir o trabalho gerando uma tabela temporária populada com parte do que se deseja da consulta original, unindo colunas que estavam em tabelas distintas, para diminuir a quantidade de JOINS. Você pode então fazer uma junção com tabelas temporárias para produzir um resultado final;
   4. Essa técnica não é muito favorável em sistemas transacionais pesados porque vai ocorrer uma sobrecarga de criação dessas tabelas temporárias, mas isso pode ser muito útil em situações de tomada de decisão (*Business Intelligence*).

### Refactoring de cursor

Quando programadores com experiência em processamento sequencial de arquivos escrevem queries, naturalmente usam cursores TRANSACT-SQL. Porém, essa técnica descarta o poder de processamento do banco de dados relacional e gera um código com uma performance insatisfatória.

Para resolver isso, existe uma técnica chamada refatoração de cursor (*cursor refactoring*), que consiste em transformar **cursores em queries** que podem melhorar o desempenho.

Cursores fornecem um mecanismo que possibilita trabalhar com uma linha ou um pequeno conjunto de linhas de uma só vez em um bloco TRANSACT-SQL. Embora sejam úteis em alguns casos, eles tipicamente usam uma grande quantidade de recursos que podem consequentemente reduzir a performance de um sistema.

Antes de aprender a técnica de refatorar um cursor, é necessário entender como criá-los e como funcionam. A construção de cursores segue um padrão em seis passos:

1. Declaração das variáveis que irão armazenar os dados que serão retornados pelo cursor;
2. Uso da sentença DECLARE CURSOR para definir a consulta que esse cursor retornará;
3. Uso da sentença OPEN para executar a consulta e popular o cursor;
4. Uso da sentença FETCH NEXT INTO para recuperar o registro da próxima linha e armazenar os valores das colunas daquela tupla nas variáveis;
5. Uso da sentença CLOSE para fechar o cursor;
6. Uso da sentença DEALLOCATE para liberar todos os recursos alocados ao cursor.

Existem quatro fatores que tornam os cursores lentos. Deste modo, recomenda-se conhecê-los para entender como contornar estes problemas na hora da refatoração:

- Cada FETCH em um cursor tem a mesma performance que uma sentença SQL. No entanto, se um cursor precisar retornar mil linhas, seria equivalente à execução do SELECT interno que o alimenta mil vezes;
- Utilizam grande quantidade de memória;
- Podem causar problemas de LOCKING no banco de dados;
- Consomem muita banda de rede quando os resultados são enviados ao cliente.

Existem algumas razões que levam os desenvolvedores a optarem por uma solução via cursor. Na maioria dos casos a escolha é feita pela facilidade da implementação, quando o correto seria desenvolver uma query com as devidas precauções com a performance. A **Tabela 1** contém uma lista de seis tarefas comuns de banco de dados para as quais cursores geralmente são usados. A última coluna informa precisamente quando optar ou não pelo seu uso.

| Tipo de problema                           | Descrição                                                    | Solução recomendada                                          | Quando usar cursores |
| ------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------- |
| Lógica complexa                            | Lógicas complexas são normalmente difíceis de refatorar e ajustar em uma solução SQL.Geralmente são desenvolvidas em uma das soluções abaixo:É feita a codificação de toda lógica em um loop de cursor;É criada uma STORED PROCEDURE que aceita um identificador, processa uma linha e retorna o valor calculado ou atualiza a linha. | Reconstruir a lógica em um SQL que utiliza a expressão CASE que torna possível manipular diversas variações de saída. | Muito raramente      |
| Iteração de código dinâmico                | É a criação de objetos de banco de dados através da linguagem DDL dinamicamente. | Para essa situação o uso de cursor é a melhor solução.       | Sempre               |
| Desnormalizando uma lista                  | É a conversão de uma lista ou conjunto de registros oriundos de diversas tabelas centralizando em apenas uma. Esta técnica é bastante utilizada para um ambiente de DW (*Data Warehouse*). | Para essa tarefa o comando SELECT INTO pode ser utilizado para criar a nova tabela desnormalizada a partir de outras fontes de dados. O mesmo pode ser feito através da sentença INSERT de um SELECT. | Algumas vezes        |
| Construindo uma consulta de tabela cruzada | A consulta do tipo tabela de referência cruzada é utilizada para sumarizar informações com a intenção de cálculo estatístico. O resultado deste tipo de consulta é apenas de leitura. Dados não podem ser adicionados, editados ou deletados. | Para construir uma consulta de tabela cruzada é necessário utilizar uma série de expressões CASE.Desde o SQL Server 2005 existe uma sintaxe chamada PIVOT, que faz efetivamente a mesma coisa que uma série de expressões CASE. | Nunca                |
| Totais cumulativos (rodando somatórias)    | São facilmente adicionados em uma ferramenta de relatório, mas algumas vezes estes totais devem ser calculados dentro do SQL Server e escritos em uma tabela.Por exemplo, quando um total cumulativo deve ser retido por razões de integridade da informação, como uma coluna de balanço financeiro. | Embora totais cumulativos possam ser obtidos utilizando uma subquery correlata, um cursor resolve melhor este problema. | Sempre               |
| Navegando em uma árvore hierárquica        | Programadores procedurais geralmente abordam essa tarefa examinando recursivamente cada nó. | Resolve-se usando STORED PROCEDURES ou funções definidas pelo usuário (*user-defined functions*) que são mais rápidas que métodos procedurais. | Nunca                |

**Tabela 1.** Relação do uso de cursores X problema.

### Refatorando um cursor

Embora sejam importantes em alguns casos, na maioria deles os cursores são desnecessários. Para prevenir o seu uso você pode implementar uma técnica de otimização conhecida como *cursor refactoring*, que consiste na reestruturação do código existente mudando sua estrutura interna sem mudar o seu comportamento externo.

*1. Desmembrando a lógica dos cursores*

Antes de refatorar um cursor você tem que descobrir o que ele faz e considerar onde e como substituí-lo por uma consulta que será mais performática que ele.

Para identificar o cerne da função de um cursor que servirá de ponto de partida para a refatoração é necessário conferir o trecho do código onde se encontra a sentença FETCH.

A **Listagem 2** mostra um cursor que retorna os nomes completos dos empregados e dos seus gerentes.

**Listagem 2.** Retornando o nome completo via CURSOR.

```
/* PARTE 1: DECLARAÇÃO DAS VARIÁVEIS DO CURSOR */
-- Declaração das variáveis que armazenarão as colunas
-- retornadas do SELECT da DECLARAÇÃO DO CURSOR
--
DECLARE  @nr_contato             INT           ,
@nr_empregado           INT           ,
@nr_gerente             INT           ,
@ds_emp_primeiro_nome   VARCHAR(50)   ,
@ds_emp_nome_do_meio    VARCHAR(50)   ,
@ds_emp_nome_de_familia VARCHAR(50)   ,
@ds_ger_primeiro_nome   VARCHAR(50)   ,
@ds_ger_nome_do_meio    VARCHAR(50)   ,
@ds_ger_nome_de_familia VARCHAR(50)   ,
@ds_emp_nome_completo   VARCHAR(100)  ,
@ds_ger_nome_completo   VARCHAR(100)  ,

/* PARTE 2: DECLARAÇÃO DO CURSOR */
-- Definição do SELECT que o cursor irá trabalhar
--
DECLARE  cr_empregado CURSOR FOR
         SELECT   tb_empregado.nr_empregado        ,
                   tb_empregado.nr_gerente          ,
                   tb_contato.ds_primeiro_nome      ,
                   tb_contato.ds_nome_do_meio       ,
                   tb_contato.ds_nome_de_familia
         FROM     db_pessoa_p.tb_contato
              JOIN db_recursos_humanos.tb_empregado
              ON   tb_contato.nr_contato
              =    tb_empregado.nr_contato

/* PARTE 3: ABERTURA DO CURSOR */
-- Neste momento recursos de memória
-- são alocados para o cursor
--
OPEN cr_empregado

/* PARTE 4: SENTENÇA FECTH */
-- Pega a primeira linha do SELECT da DECLARAÇÃO DO CURSOR
-- E armazena nas variáveis declaradas na PARTE 1
--
FETCH NEXT FROM cr_empregado
INTO  @nr_empregado              ,
      @nr_gerente            ,
      @ds_emp_primeiro_nome  ,
      @ds_emp_nome_do_meio   ,
      @ds_emp_nome_de_familia

-- Enquanto tiver linha a ser retornada pelo SELECT
-- da DECLARAÇÃO DO CURSOR, faça...
--
WHILE @@FETCH_STATUS = 0
  BEGIN
  SELECT
    @ds_ger_primeiro_nome   = tb_contato.ds_primeiro_nome,
    @ds_ger_nome_do_meio    = tb_contato.ds_nome_do_meio ,
    @ds_ger_nome_de_familia = tb_contato.ds_nome_de_familia
  FROM db_pessoa_p.tb_contato
         JOIN db_recursos_humanos.tb_empregado
         ON   tb_contato.nr_contato
         =    tb_empregado.nr_contato
  WHERE  tb_empregado.nr_empregado = @nr_gerente

  -- Se o SELECT acima não retornar nenhuma linha, faça...
  IF @@ROWCOUNT = 0
     BEGIN
       SET @ds_emp_nome_completo = @ds_emp_primeiro_nome
           + ISNULL(' '+@ds_emp_nome_do_meio+'. ', ' ')
           + @ds_emp_nome_de_familia
       SET @ds_ger_nome_completo = NULL
     END
  ELSE
     BEGIN
       SET @ds_emp_nome_completo = @ds_emp_primeiro_nome
           + ISNULL(' '+@ds_emp_nome_do_meio+'. ', ' ')
           + @ds_emp_nome_de_familia
       SET @ds_ger_nome_completo = @ds_ger_primeiro_nome
          + ISNULL(' '+@ds_ger_nome_do_meio+'. ', ' ')
          + @ds_ger_nome_de_familia
       END
  SELECT  @ds_emp_nome_completo AS Nome_do_Empregado,
         @ds_ger_nome_completo AS Nome_do_Gerente

  FETCH NEXT FROM cr_empregado
  INTO  @nr_empregado            ,
        @nr_gerente                   ,
        @ds_emp_primeiro_nome         ,
        @ds_emp_nome_do_meio     ,
        @ds_emp_nome_de_familia
END -– Para fechar o WHILE
CLOSE      cr_empregado –-PARTE 5:FECHANDO O CURSOR
DEALLOCATE cr_empregado –-PARTE 6:DESALOCANDO O CURSOR
```

*2. Reconstruindo a lógica em múltiplas consultas*

Ainda mantendo o cursor da **Listagem 2**, mas melhorando sua performance, a **Listagem 3** mostra uma possível solução para reconstruí-lo de uma forma mais simples. Para isso, é necessário remover algumas variáveis e transformar parte do código que busca linha por linha em duas queries.

**Listagem 3.** Melhorando o cursor com múltiplas consultas.

```
-- Nova lógica da query de população do cursor
SELECT tb_empregado.nr_empregado,
       tb_empregado.nr_gerente  ,
       tb_contato.ds_primeiro_nome
       + ISNULL(' '+tb_contato.ds_nome_do_meio+'. ', ' ')
       + tb_contato.ds_nome_de_familia AS Nome_Completo
FROM   db_pessoa_p.tb_contato
 JOIN  db_recursos_humanos.tb_empregado
   ON  tb_contato.nr_contato
    =  tb_empregado.nr_contato

-- Nova consulta da sentença WHILE
SELECT @ds_ger_nome_completo = tb_contato.ds_primeiro_nome
       + ISNULL(' '+tb_contato.ds_nome_do_meio+'. ', ' ')
       + tb_contato.ds_nome_de_familia
FROM   db_pessoa_p.tb_contato
 JOIN  db_recursos_humanos.tb_empregado
   ON  tb_contato.nr_contato
    =  tb_empregado.nr_contato
WHERE  tb_empregado.nr_empregado = @nr_gerente
```

*3. Reconstruindo a lógica em uma função definida pelo usuário*

A **Listagem 4** mostra outra solução. Neste exemplo o cursor da **Listagem 3** será totalmente substituído por uma função definida pelo usuário (*user-defined function*) que retornará o mesmo resultado do cursor de uma forma mais eficaz.

**Listagem 4.** Reconstruindo o cursor com uma função definida pelo usuário.

```
-- Função que recebe o número da matrícula do gerente
-- e retorna o seu nome completo
CREATE FUNCTION dbo.NomeCompletoGerente

-- Recebe a variável
-- Número da matrícula do gerente do tipo inteiro
( @nr_gerente INT )

-- Informa que o retorno da função será um VARCHAR(100)
RETURNS VARCHAR (100)
AS
BEGIN
  -- Declaração da variável que receberá
  -- os dados do SELECT
  DECLARE @GerNomeCompleto VARCHAR(100)

  -- Seleciona o nome completo do gerente
  -- e insere na variável @GerNomeCompleto
  SELECT
        @GerNomeCompleto = tb_contato.ds_primeiro_nome
        + ISNULL(' '+tb_contato.ds_nome_do_meio+'. ', ' ')
        + tb_contato.ds_nome_de_familia
  FROM  db_pessoa_p.tb_contato
  JOIN  db_recursos_humanos.tb_empregado
    ON  tb_contato.nr_contato
     =  tb_empregado.nr_contato
  WHERE tb_empregado.nr_empregado = @nr_gerente

  -- Caso a consulta não retorne nenhuma linha, faça...
  IF @@ROWCOUNT = 0
     BEGIN
       SET @GerNomeCompleto = NULL
     END
  -- Exibe o nome complete do gerente
  RETURN @GerNomeCompleto
END
```

*4. Reconstruindo a lógica em uma consulta complexa*

O passo mais avançado na refatoração é transformar completamente um cursor em um código SQL complexo, como exemplificado na **Listagem 5.**

O objetivo é substituir a função definida pelo usuário, criada na **Listagem 4**, por uma codificação mais rebuscada, que visa extrair do processador de consultas o maior ganho de performance possível através de um código.

**Listagem 5.** Reconstruindo o cursor com uma consulta complexa.

```
--
SELECT    CtEmp.ds_primeiro_nome
          + ISNULL(' '+ CtEmp.ds_nome_do_meio+'. ', ' ')
          + CtEmp.ds_nome_de_familia AS Nome_Completo_Emp,
          CtGer.ds_primeiro_nome
          + ISNULL(' '+ CtGer.ds_nome_do_meio+'. ', ' ')
          + CtGer.ds_nome_de_familia AS Nome_Completo_Ger
FROM      db_pessoa_p.tb_contato AS CtEmp
JOIN      db_recursos_humanos.tb_empregado
  ON      CtEmp.nr_contato = tb_empregado.nr_contato
LEFT JOIN db_recursos_humanos.tb_empregado AS Gerente
       ON Gerente.nr_empregado = tb_empregado.nr_gerente
LEFT JOIN db_pessoa_p.tb_contato AS CtGer
       ON CtGer.nr_contato = Gerente.nr_contato
```

### Usando as expressões PIVOT e CTE

Desde o SQL Server 2005 foram fornecidas duas funcionalidades muito úteis que ajudam na refatoração de um cursor. Uma é a CTE (*Common Table Expression*) e a outra é a sentença PIVOT. Estas expressões podem ser utilizadas individualmente ou juntas, visando diminuir a quantidade de código que precisa ser reescrito.

CTE é um resultado temporário que você define dentro de uma sentença SQL. Essa expressão é similar às tabelas temporárias e tabelas derivadas por serem montadas em tempo de execução. Elas permitem a criação de consultas aninhadas e recursivas, utilizando uma sintaxe mais legível do que qualquer outro recurso.

Na sua sintaxe as CTEs são definidas por um nome, uma lista de colunas entre parêntesis (opcional), e de uma consulta SQL. Depois de serem definidas, elas podem ser referenciadas nas sentenças: SELECT, INSERT, UPDATE, DELETE ou CREATE VIEW. Dentro de um bloco de código as CTEs podem ser referenciadas diversas vezes mediante a necessidade do desenvolvedor.

A **Listagem 6** mostra o exemplo de uma CTE em um bloco de código que tem como result set informações de vendas.

**Listagem 6.** Seleção de vendas utilizando CTE.

```
WITH Vendas_CTE (nr_vendedor, nr_pedidos, dt_ult_pedido)
AS
  (
    SELECT nr_vendedor      ,
          COUNT(*)          ,
          MAX(dt_pedido)    ,
    FROM  db_vendas.tb_pedido
  )
SELECT E.nr_empregado  ,
       OS.nr_pedidos   ,
       OS.dt_ult_pedido
FROM   db_recursos_humanos.tb_empregado AS E
  JOIN Vendas_CTE AS OS
    ON E.nr_empregado = OS.nr_vendedor
    LEFT OUTER JOIN Vendas_CTE AS OM
    ON E.nr_gerente = OM.nr_vendedor
ORDER BY E.nr_empregado;
```

A outra funcionalidade é o comando PIVOT. Este permite que você crie uma visão de dados de uma consulta de tabela cruzada utilizando uma simples e legível sintaxe, ao invés de ter que escrever uma série de sentenças CASE.

Para entendermos como funciona esta sentença vamos preparar um pequeno ambiente para testarmos como o mesmo problema era solucionado no Microsoft SQL Server 2000 – antes de existir a sentença PIVOT – e na versão 2005, quando ela surgiu.

A **Listagem 7** contém o código DDL para criação da tabela de trabalho **tb_venda**, seguido do código DML para inserção dos registros.

**Listagem 7.** Criando a tabela tb_venda.

```
CREATE TABLE tb_venda
( ds_ano      INT         NOT NULL,
  ds_mes      INT         NOT NULL,
  ds_valor    NUMERIC(9,2) NOT NULL,
)
GO
INSERT INTO tb_venda VALUES (2003, 2, 10)
INSERT INTO tb_venda VALUES (2003, 2, 1)
INSERT INTO tb_venda VALUES (2003, 3, 20)
INSERT INTO tb_venda VALUES (2003, 4, 30)
INSERT INTO tb_venda VALUES (2004, 1, 40)
INSERT INTO tb_venda VALUES (2004, 2, 50)
INSERT INTO tb_venda VALUES (2004, 3, 60)
INSERT INTO tb_venda VALUES (2004, 4, 70)
INSERT INTO tb_venda VALUES (2005, 1, 80)
GO
```

Executando um SELECT na tabela criada na **Listagem 7** é obtido o resultado exibido na **Tabela 2**. Este resultado é fundamental para a conclusão do entendimento da funcionalidade do comando PIVOT.

|      | ds_ano | ds_mes | ds_valor |
| ---- | ------ | ------ | -------- |
| 1    | 2008   | 2      | 1.00     |
| 2    | 2008   | 2      | 10.00    |
| 3    | 2008   | 3      | 20.00    |
| 4    | 2008   | 4      | 30.00    |
| 5    | 2009   | 1      | 40.00    |
| 6    | 2009   | 2      | 50.00    |
| 7    | 2009   | 3      | 60.00    |
| 8    | 2009   | 4      | 70.00    |
| 9    | 2010   | 1      | 80.00    |

**Tabela 2.** Consultando todas as linhas de tb_venda.

Para gerar um resultado de pivotamento de tabela, na versão 2000 do Microsoft SQL Server era necessário utilizar uma função de grupo através da sentença CASE mais a cláusula GROUP BY (ver **Listagem 8**).

**Listagem 8.** Pivotamento de tabela através da expressão CASE.

```
SELECT   ds_ano as Ano,
    Jan = sum(case when ds_mes=1 then valor end),
    Fev = sum(case when ds_mes=2 then valor end),
    Mar = sum(case when ds_mes=3 then valor end),
    Abr = sum(case when ds_mes=4 then valor end)
FROM     tb_venda
GROUP BY ds_ano
ORDER BY ds_ano
```

A **Tabela 3** exibe o resultado da consulta da **Listagem 8**.

|      | Ano  | Jan   | Fev   | Mar   | Abr   |
| ---- | ---- | ----- | ----- | ----- | ----- |
| 1    | 2008 | NULL  | 11.00 | 20.00 | 30.00 |
| 2    | 2009 | 40.00 | 50.00 | 60.00 | 70.00 |
| 3    | 2010 | 80.00 | NULL  | NULL  | NULL  |

**Tabela 3.** Resultado da consulta CASE

A **Listagem 9** mostra a utilização do comando PIVOT para realizar o pivotamento de uma tabela com valores sumarizados por mês e por ano. O objetivo deste exemplo é servir de comparação com o script da **Listagem 8** (que mostra a única solução na época quando não existia o comando PIVOT) para percebemos as diferenças de dois códigos com a mesma intenção de pivotamento.

**Listagem 9.** Pivotamento de tabela através da expressão PIVOT.

```
SELECT   ds_ano as Ano,
         [1]    as Jan,
    [2]    as Fev,
    [3]    as Mar,
    [4]    as Abr
FROM     tb_venda
PIVOT   (SUM(ds_valor) FOR ds_mes IN ([1],[2],[3],[4])) p
ORDER BY 1
```

A **Tabela 4** exibe o resultado obtido pela execução do código da **Listagem 9**. Observe que o RESULT SET é exatamente o mesmo da **Tabela 3**.

|      | Ano  | Jan   | Fev   | Mar   | Abr   |
| ---- | ---- | ----- | ----- | ----- | ----- |
| 1    | 2008 | NULL  | 11.00 | 20.00 | 30.00 |
| 2    | 2009 | 40.00 | 50.00 | 60.00 | 70.00 |
| 3    | 2010 | 80.00 | NULL  | NULL  | NULL  |

**Tabela 4.** Resultado da consulta com a expressão PIVOT.

### Otimizando uma estratégia de indexação

Ao realizar o tunning de um banco de dados, um dos principais recursos para obtenção de uma melhora imediata na lentidão de um sistema é o uso apropriado de índices.

Os índices são a ponte de comunicação entre os dados e a consulta, e eles habilitam o processador de consultas para eficientemente encontrar os dados que você precisa.

Antes de entrarmos no entendimento do que são índices, como eles funcionam e quando devem ser criados, primeiramente temos que compreender como os dados são armazenados e acessados.

### Como os dados são armazenados

O SQL Server armazena os dados em páginas independentemente do tipo de entidade, portanto, essa regra serve tanto para tabela com índice clusterizado (*clustered index*) quanto para HEAP TABLE (tabela não ordenada por índice clusterizado).

Alguns conceitos são necessários para a compreensão do assunto:

1. Páginas de dados (*data pages*): Os registros são armazenados em páginas de dados;

2. 1. Cada página de dados contém 8 KB (*kilobytes*) de informação. Um grupo de oito páginas adjacentes é chamado de um EXTENT;
   2. Quando uma linha é inserida em uma página que já se encontra cheia, a página é dividida e metade das linhas é movida para a nova página.

1. HEAPS: Uma heap é uma coleção de páginas de dados de uma tabela que não possui um índice clusterizado;

2. 1. As linhas de dados não são armazenadas em uma ordem particular, e não existe nenhuma ordem na sequência das páginas de dados.

1. Índice Clusterizado: Conhecido também como índice agrupado, este tipo é criado automaticamente por DEFAULT na definição de uma chave primária quando não for especificado o tipo de índice desejado (CLUSTERED, que é o default, e NONCLUSTERED) em sua criação;

2. 1. As linhas de dados são fisicamente armazenadas em uma ordem baseada no índice da chave clusterizada;
   2. Tanto a ordenação física das linhas da tabela quanto a ordem das linhas no índice são as mesmas.

1. Estrutura B-Tree (Árvore binária): É uma estrutura de dados com um conjunto finito de elementos denominados vértices ou nós.

2. 1. O SQL Server utiliza o mesmo princípio da lista telefônica, gravando as informações dos índices em uma estrutura chamada **B-Tree**. Uma estrutura B-Tree possui um nó-raiz que contém uma única página de dados, uma ou mais páginas de níveis intermediários e uma ou mais páginas de níveis folha. A **Figura 1** mostra um exemplo desta estrutura.

![Estrutura B-Tree](https://arquivo.devmedia.com.br/REVISTAS/sql/imagens/86/4/image001.jpg)**Figura 1**. Estrutura B-Tree.

### Como os dados são acessados

O SQL Server acessa os dados de duas maneiras:

1. Percorrendo todas as páginas de dados da tabela (chamado TABLE SCAN). Essa varredura é realizada da seguinte forma:

2. 1. Começa do início da tabela;
   2. Varre página a página através de todas as linhas da tabela;
   3. Extrai as linhas que atendem ao critério da consulta.

1. Utilizando índices. Esse processo ocorre da seguinte forma:

2. 1. Percorre a estrutura da árvore do índice para encontrar as linhas solicitadas pela query;
   2. Extrai somente as linhas necessárias que atendem ao critério da consulta.

Ao realizar uma consulta, o SQL Server primeiramente analisa a existência de um índice. Então o otimizador de consultas – componente responsável por gerar o melhor plano de execução para uma query – determina se é mais eficaz varrer toda a tabela ou se utiliza o índice para acessar os dados.

O SQL Server usa índices para facilitar a busca de informações em uma tabela com o menor número possível de operações de leitura, tornado assim a busca mais rápida e eficiente. Com os dados predispostos em uma estrutura de árvore, o otimizador de consultas precisa apenas interrogar um pequeno bloco de linhas de uma tabela para resolver os predicados e as condições de JOIN.

O uso apropriado de índices reduz substancialmente as operações de I/O e memória, portanto, melhora as consultas do banco de dados e o desempenho geral do banco como um todo. Contudo, uma estratégia de indexação ineficiente pode afetar negativamente a performance de um sistema.

### Tipos de índice

O processador de consultas relacional do Microsoft SQL Server utiliza árvores binárias (*B-trees*) para a sua estrutura de índice. Os tipos disponíveis de índices são:

1. Índice clusterizado (*clustered index*);

2. 1. Ele reorganiza a ordem física dos dados na tabela em que foi criado;
   2. Só é possível criar um índice deste tipo por tabela, porque os dados de uma tabela só podem ser ordenados em apenas um sentido;
   3. Por padrão todas as chaves primárias criadas no SQL Server utilizam índices clusterizados.

1. Índice não clusterizado (*non-clustered index*);

2. 1. Ele armazena uma cópia dos dados usados para definir sua chave e reorganiza a ordem física desta cópia de dados;
   2. Além disso, se existir um índice clusterizado na tabela, o índice não clusterizado também armazena os valores da chave clusterizada;
   3. Caso a tabela seja uma HEAP, ou seja, se não tiver um índice clusterizado, o localizador de linha será um ponteiro para a linha. O ponteiro é criado a partir do ID (identificador) do número da página e do número da linha na página do arquivo. O ponteiro inteiro é conhecido como RID (Identificação de Linha);
   4. Por usar uma cópia dos dados, em uma tabela podem ser criados mais de um índice deste tipo;
   5. Por padrão, UNIQUE CONSTRAINTS criadas no SQL Server utilizam índices não clusterizados.

1. Índice único (*unique index*);

2. 1. Ele garante que a chave do índice não conterá valores duplicados, consequentemente todas as linhas da tabela ou da visão são únicas;
   2. Tanto índices clusterizados como não clusterizados podem ser únicos.

1. Índice com colunas inclusas (*index with included columns*);

2. 1. É um índice não clusterizado que tem adicionado às colunas chaves outras colunas que não fazem parte da chave.

1. Visão indexada (*indexed views*);

2. 1. Uma visão indexada tem seu resultado permanentemente armazenado em um índice clusterizado único;
   2. Somente depois de ser criado um índice clusterizado na VIEW é que pode ser adicionado outro não clusterizado.

1. Índice de texto completo (*full-text index*);

2. 1. É um tipo especial de índice funcional baseado em símbolos;
   2. Fornece suporte eficiente para sofisticadas buscas por palavras em linhas de caracteres de dados.

1. Índice XML.

2. 1. É uma representação de grandes objetos XML binários em uma coluna do tipo XML.

### Diretrizes para se criar índices

Existem alguns cuidados que devem ser tomados na hora da criação de um índice, pois se as escolhas de indexação forem mal planejadas a intenção de tornar o sistema mais rápido pode ficar comprometida.

1. Examine as características do banco de dados;

2. 1. A sua estratégia de indexação irá diferir, por exemplo, entre um sistema com transações online com atualizações frequentes e um sistema de DATAWAREHOUSE que contém somente dados de leitura.

1. Entenda as características das consultas frequentemente usadas e das colunas utilizadas nas queries;

2. 1. Por exemplo, você pode precisar criar um índice em uma consulta que utiliza joins ou que usa uma coluna única no seu argumento de busca (na cláusula WHERE).

1. Quando um índice é criado ou recriado, é possível escolher algumas opções. Decida qual delas irá prover melhor performance;

2. 1. As opções que podem afetar a eficiência de um índice são: FILLFACTOR e ONLINE.

1. Determine o melhor local de armazenamento para o índice;

2. 1. Você pode escolher entre armazenar índices não clusterizados no mesmo grupo de arquivos (*filegroup*) que estão armazenados os dados (tabela) ou em outro diferente;
   2. Se os índices forem armazenados em um FILEGROUP que está em um disco diferente do HD de dados, a performance de I/O de disco será bem melhor porque múltiplos discos podem realizar leituras ao mesmo tempo.

1. Balanceie a performance de escrita e leitura no banco de dados;

2. 1. Índices não clusterizados podem ser criados em uma tabela, mas é importante lembrar que a cada novo índice criado há um impacto na performance das operações de inserção e atualização;
   2. Isso ocorre porque os índices não clusterizados mantêm cópias dos dados, e cada cópia destas requerem operações de I/O, podendo causar uma redução no desempenho de escrita se o banco tiver que escrever muitas cópias;
   3. Ao fazer uma estratégia de indexação é preciso examinar a quantidade de operações de consulta (*selects*) e atualização (*updates*) para balancear de acordo com a necessidade do sistema.

1. Considere o tamanho das tabelas no banco de dados;

2. 1. O processador de consultas pode demorar mais para varrer um índice de uma tabela pequena do que realizar um TABLE SCAN. Consequentemente, mediante a esta avaliação, o processador de consultas nunca usará este índice, contudo, este índice continuará sendo atualizado quando houver mudança nos dados da tabela.

1. Considere o uso de visões indexadas (*indexed views*).

2. 1. Podem garantir um ganho significativo de performance quando existirem agregações ou joins de tabela na VIEW.

### Índices não clusterizados

Utilize estes índices para aumentar a performance de consultas que não usam o índice clusterizado. Diferentemente dos índices clusterizados, que reorganizam a própria tabela, os índices não clusterizados organizam somente os dados que foram especificamente incluídos no índice.

Índices são armazenados em páginas, e quando o processador de consultas faz uma leitura dos dados no disco, ele lê uma página inteira de uma só vez. Os índices não clusterizados geralmente contêm menos dados que os clusterizados, sendo assim, podem caber mais registros em uma página.

Uma leitura de dados feita utilizando este tipo de índice requer menos operações de I/O do que uma feita através de um índice clusterizado. Por esta razão, este índice é a melhor escolha quando poucas colunas são usadas em uma query.

### Quando utilizar índices não clusterizados

Este tipo de índice deve ser criado quando existirem colunas na query envolvidas em:

- Predicados;
- Joins;
- Agregações.

Quando uma query é executada o otimizador de consultas varre o índice não clusterizado com o objetivo de encontrar o local dos dados que estão dentro da tabela e então retorna os valores requeridos na consulta para o solicitante. Isso faz com que estes índices sejam a melhor escolha para consultas com colunas que tenham as funções acima mencionadas.

Depois que o otimizador de consultas encontra todas as entradas no índice, ele pode ir diretamente à página e linha exatas para retornar os dados.

O índice não clusterizado atende uma consulta se no índice contiver todas as colunas utilizadas na query. Neste sentido, a performance vai ser bastante similar a uma consulta que utiliza índice clusterizado.

### Índices com colunas inclusas

Desde a versão 2005, o SQL Server fornece a possibilidade de incluir colunas não chave às colunas chave de um índice não clusterizado.

As colunas não chave são armazenadas no último nível da árvore binária de um índice. Esse tipo de índice é muito parecido com um não clusterizado quando este possuir todas as colunas da consulta, porém nele há um ganho maior de desempenho porque as páginas de índices não são movidas se forem realizadas atualizações nas colunas não chave.

A **Listagem 10** mostra a sintaxe para criar um índice com colunas inclusas.

**Listagem 10.** Criação de um índice com colunas inclusas.

```
CREATE NONCLUSTERED INDEX idx_empregado
ON db_recursos_humanos.tb_empregado (nr_empregado)
INCLUDE (ds_endereco, nr_telefone, nr_CPF)
```

### Cuidados a serem evitados na criação de um índice não clusterizado

Existem alguns erros de indexação que podem levar a um sistema ineficiente, afetando o desempenho do banco de dados. Vejamos alguns deles:

\1. **Índices redundantes:** São aqueles criados desnecessariamente, com colunas duplicadas em outros. Consomem espaço de disco e também afetam o desempenho de manutenção do índice;

\2. **Índices compostos por várias colunas:** Forçam o processador de consultas a realizar buscas em todas as colunas do índice;

\3. **Índices para uma consulta:** Antes de criar um índice para atender a apenas uma consulta, deve ser considerado se este pode atender outras queries para evitar consumo de espaço físico;

\4. **Índices não clusterizados que incluem índices clusterizados:** Não é necessário adicionar as colunas de um índice clusterizado ao final das colunas de um índice não clusterizado porque elas são incluídas automaticamente.

### Quando utilizar índices clusterizados

Existem alguns tipos de consultas que são candidatas potenciais a utilizarem este tipo de índice para obter um ganho maior de desempenho. Dentre elas podemos citar:

1. Consultas de escala;

2. 1. Esse tipo de consulta, como aquelas que contêm coluna de data, são exigidas comumente em sistemas transacionais e multidimensionais;
   2. Geralmente envolve a necessidade de selecionar todos os dados de certo período;
   3. O processador de consultas pode usar um índice clusterizado para eficientemente retornar páginas de dados baseados em um escopo e ler todos os dados das colunas no BUFFER CACHE;
   4. A razão para isso se dá porque depois que o processador de consultas encontra a linha com o primeiro valor do escopo, as linhas com valores subsequentes irão ficar adjacentes fisicamente no índice clusterizado.

1. Consultas de chave primária;

2. 1. Podem causar gargalos na performance se a chave primária não for um índice clusterizado. Neste caso, o processador de consultas irá realizar uma procura utilizando a chave para obter as outras colunas requisitadas pela query;
   2. Para evitar um custo alto nas operações de procura, transforme a chave primária para utilizar índice clusterizado ou crie um índice que tenha todas as colunas da consulta.

1. Consultas que retornam dados de várias colunas.

2. 1. Quando o processador de consultas lê os dados do disco, ele sempre lê uma página inteira onde estão os registros solicitados na query, mesmo que estes não ocupem toda a página;
   2. Como dito anteriormente, índices clusterizados são, na realidade, reorganizações físicas dos dados físicos da tabela;
   3. Consequentemente, quando o processador de consultas lê as páginas de dados utilizando a chave de um índice clusterizado, ele simultaneamente lê todas as colunas associadas na query;
   4. Neste sentido, você deve criar índices clusterizados em chaves que são usadas em uma consulta para retornar os dados.

Baseado nas informações mencionadas acima, você deve considerar criar índices clusterizados em colunas que são:

- Únicas ou contêm muitos valores distintos;
- Acessadas com valores sequenciais;
- Usadas como chave estrangeira (*foreign key*);
- Usadas frequentemente para organizar os dados retornados da tabela.

### Quando não usar índices clusterizados

Você deve considerar não usar esse tipo de índice nas seguintes situações:

1. Quando colunas sofrem mudanças constantes;

2. 1. Alterações em uma coluna que possui um índice clusterizado causam a movimentação de toda a linha, pois o processador de consultas mantém os valores dos dados da linha em uma ordem física. Isso pode causar problemas de performance em sistemas que têm dados voláteis;

1. Quando estiver usando chaves com muitas colunas.

2. 1. Chaves grandes consistem em muitas colunas. Já os Índices não clusterizados usam os valores da chave do índice clusterizado como chaves de pesquisa;
   2. Se o índice clusterizado tem uma chave grande, o índice não clusterizado também terá um tamanho significativamente grande. Isso ocorre porque nele estão as várias colunas da chave clusterizada, assim como as colunas do próprio índice clusterizado.

### Como documentar uma estratégia de indexação

Índices são fundamentais para garantir um banco de dados com alta performance, contudo, você achará difícil decidir quais índices deverão ser criados e como documentar este processo de decisão.

Existe um método que pode ser utilizado para garantir uma boa prática na estratégia de indexação que visa facilitar a documentação e escolha adequada para a criação de um índice. Este método é conhecido como planilha de estratégia de indexação.

Esta planilha deverá conter uma linha para cada coluna de uma tabela e uma coluna para cada STORED PROCEDURE existente. Após montar a planilha, sinalize com um X cada coluna de tabela que possuir uma SP que utiliza aquela coluna.

Adote uma codificação para sinalizar na planilha que aquela SP utiliza a coluna em uma condição de JOIN, ou em um predicado ou na lista do SELECT. Então você pode utilizar essas informações da planilha de estratégia de indexação para orientar na tomada de decisão na hora de criar um índice.

Dessa forma, com uma rápida leitura desta planilha fica fácil saber quais índices devem ser criados para melhorar o desempenho. Assim, você pode definir os índices por um processo de eliminação. A partir disso, considere criar índices clusterizados para as colunas que você marcou como bastante utilizadas em predicados ou em condições de JOIN. Para aquelas colunas menos utilizadas em predicados e em condições de JOIN, pondere criar índices não clusterizados. Finalmente, considere criar um índice se várias consultas utilizam repetidamente o mesmo grupo de colunas.

A **Figura 2** exemplifica o preenchimento da planilha de estratégia de indexação que serve de orientação para a criação de índices por sistema.

![Planilha de estratégia de indexação](https://arquivo.devmedia.com.br/REVISTAS/sql/imagens/86/4/image002.jpg)**Figura 2**. Planilha de estratégia de indexação.

### Gerenciando concorrência

A essência de um Sistema Gerenciador de Bancos de Dados é armazenar e gerenciar registros que geralmente são compartilhados, sendo aptos a atenderem requisições de diversos usuários ao mesmo tempo. Contudo, a habilidade para acessar e modificar concorrentemente o mesmo registro tem um custo.

Se outro usuário estiver modificando um registro que você precise, você terá que esperá-lo terminar para ter acesso a esse dado. O SQL Server implementa essa funcionalidade com o uso de LOCKS que bloqueiam o registro para garantir que os outros usuários vejam apenas o dado correto.

Para criar uma aplicação com um rápido desempenho de resposta, você deve gerenciar a alocação de LOCKS e também qualquer bloqueio que ocorra.

Os níveis de isolamento de transação definem o grau ao qual uma transação deve ser isolada na hora de uma modificação de dados feita por outras transações. Esses níveis ajudam no gerenciamento quando ocorrem efeitos secundários (veja a **Nota DevMan 1**) em transações concorrentes, como dependências não confirmadas, isto é, leituras sujas ou leituras fantasmas.

Os níveis de isolamento de transações servem para controlar as seguintes situações:

1. Quando estiver ocorrendo LOCK durante o acesso a um registro;
2. Controlar o tempo que um registro está bloqueado durante um LOCK de leitura;
3. Se operações de leitura estiverem referenciando registros modificados por outra transação:
4. Bloqueia o registro até que o LOCK exclusivo seja liberado;
   1. Recupera a versão confirmada do registro que existia no momento em que foi iniciada a transação;
   2. Lê as modificações de registros não confirmados.

Um baixo nível de isolamento aumenta a possibilidade de vários usuários acessarem um registro ao mesmo tempo, e também aumenta a quantidade dos efeitos de concorrência (como leituras sujas ou atualizações perdidas) que os usuários irão se deparar.

Inversamente, um alto nível de isolamento reduz alguns tipos de efeitos de concorrência que usuários podem encontrar, porém exigem mais recursos do sistema e aumenta a chance de uma transação bloquear a outra.

Ao escolher um nível de isolamento apropriado, deve ser posto na balança o custo entre as requisições de integridade de dados da aplicação e o nível de isolamento escolhido.

O mais alto nível de isolamento (*serializable*) garante que a transação irá recuperar exatamente o mesmo registro toda vez que repetir uma operação de leitura. Contudo, essa garantia requer um nível de bloqueio que irá afetar outros usuários em sistemas multiusuários.

O mais baixo nível de isolamento (*read uncommitted*) recupera os registros que outras transações modificaram, mas não confirmaram. Este nível minimiza o custo do sistema porque não existe bloqueio de leitura nem versionamento de registro, por outro lado permite a leitura suja de dados.

Os níveis de isolamento existentes são:

1. **Leitura não confirmada (\*Read uncommitted\*):** Este é o nível menos restritivo e especifica que as requisições podem ler o que outras transações modificaram, mesmo que o registro não tenha sido confirmado. As transações são isoladas somente o suficiente para garantir que dados corrompidos fisicamente não sejam lidos. Neste nível são prevenidas as atualizações perdidas, mas podem ocorrer leituras sujas, leituras não repetíveis e leituras fantasmas;

2. **Leitura confirmada (\*Read committed\*):** Este é o nível default do banco de dados. Embora possam ocorrer leituras não repetíveis e fantasmas, as leituras sujas não acontecem. No SQL Server 2005 existe uma nova implementação neste nível – quando a opção READ_COMMITTED_SNAPSHOT for configurada para ON.

3. 1. Este nível usará versionamento para ler a última versão confirmada do registro se este estiver bloqueado exclusivamente por outro usuário.
   2. Operações de leitura requerem apenas o nível de bloqueio de tabela SCH-S (veja a **Nota DevMan 1**) (veja a **Nota DevMan 2**) e nenhum bloqueio de registro ou página.
   3. Quando a opção READ_COMMITTED_SNAPSHOT do banco estiver configurada para OFF, que é o default, o comportamento continua o mesmo das versões anteriores do SQL Server para o isolamento de leituras confirmadas;

4. **Leitura Repetível (\*Repeatable read\*):** Esse nível especifica que requisições não podem ler o registro que outra transação modificou, mas não confirmou. Significa também que outra transação não pode modificar dados que foram lidos pela transação atual até que ela seja completada. Não ocorrem leituras sujas e não repetíveis; contudo, leituras fantasmas podem ocorrer.

5. 1. Devido aos bloqueios compartilhados serem mantidos até que se encerre a transação, em vez de ser liberado ao término de cada sentença SQL, a concorrência é mais baixa que o nível de isolamento READ COMMITED. Utilize esse nível somente quando for necessário;

6. **Instantâneo (\*Snapshot\*):** Este foi um nível de isolamento que surgiu no SQL Server 2005. Utiliza versionamento de registro para fornecer consistência de leitura no nível de transação.

Operações de leitura não recebem bloqueios de registros ou páginas; ocorre somente o bloqueio de tabela do tipo SCH-S. Quando ocorrer leitura de registros modificados por outra transação, ele recupera a versão do registro que existia quando foi iniciada a transação.

Geralmente, transações SNAPSHOT não requerem bloqueios quando os dados estão sendo lidos. Leitura de registros nesse tipo de isolamento não bloqueiam outras transações que estão gravando registros, e as transações que estiverem gravando registros não bloqueiam transações SNAPSHOT que estejam lendo registros. Leituras sujas, não repetíveis, e fantasmas não ocorrem neste nível.

Transações que rodam sob este nível de isolamento que selecionam registros para atualização não recebem bloqueios.

Quando uma linha de dados encontra um critério de atualização, a transação SNAPSHOT verifica se o registro não foi modificado por uma transação concorrente que foi confirmada depois que a transação SNAPSHOT começou. Se a linha foi modificada por uma transação externa à SNAPSHOT, ocorre um conflito de atualização e a transação SNAPSHOT é finalizada.

Um custo que existe neste tipo de isolamento é que ele faz bastante uso do banco tempdb. Cada vez que um registro é modificado por uma transação, a instância do SGBD armazena a imagem da versão previamente confirmada do registro no tempdb. Cada versão é marcada com um número sequencial da transação que fez a modificação.

A **Listagem 11** mostra a sintaxe para configurar este nível de isolamento em um banco chamado db_teste.

**Listagem 11.** Configurando o nível de isolamento SNAPSHOT.

```
ALTER DATABASE db_teste
SET ALLOW_SNAPSHOT_ISOLATION ON
GO
SET TRANSACTION ISOLATION LEVEL SNAPSHOT
```

1. **Serializável (\*Serializable\*):** Esse nível garante que as transações sejam completamente isoladas uma da outra. Este é o mais restritivo nível de isolamento porque ele bloqueia conjuntos de chaves inteiras (compostas) e segura o bloqueio até que a transação termine. Embora não ocorram leituras sujas, não repetíveis e fantasmas, a concorrência é baixa, portanto, use este nível de isolamento somente quando necessário.

A **Tabela 5** mostra os efeitos colaterais de simultaneidade habilitados por níveis de isolamento diferentes.

|                        | Tipo de leitura |                       |          |
| ---------------------- | --------------- | --------------------- | -------- |
| Nível de isolamento    | Leitura suja    | Leitura não repetível | Fantasma |
| Leitura não confirmada | Sim             | Sim                   | Sim      |
| Leitura confirmada     | Não             | Sim                   | Sim      |
| Leitura repetível      | Não             | Não                   | Sim      |
| Instantâneo            | Não             | Não                   | Não      |
| Serializável           | Não             | Não                   | Não      |

**Tabela 5.** Níveis de isolamento por tipo de leitura.



**Nota: Efeitos de simultaneidade**



Existem alguns tipos de efeito de simultaneidade que ocorrem quando duas ou mais transações estão disputando o mesmo registro, aos quais podemos citar:

\1. **Atualizações perdidas**: As atualizações perdidas acontecem quando duas ou mais transações selecionam a mesma linha e então atualizam a linha com base no valor selecionado originalmente.

Cada transação não tem conhecimento das outras. Assim, a última atualização substitui atualizações feitas pelas outras transações, o que resulta em dados perdidos.

Por exemplo, dois editores fazem uma cópia eletrônica do mesmo documento. Cada editor altera a cópia de maneira independente e salva a cópia alterada, substituindo, portanto, o documento original.

O editor que salva a cópia alterada por último substitui as alterações feitas pelo outro editor. Esse problema poderia ser evitado se um editor não pudesse acessar o arquivo até que o outro tivesse terminado e confirmado a transação;

\2. **Dependência não confirmada (leitura suja)**: A dependência não confirmada acontece quando uma segunda transação seleciona uma linha que está sendo atualizada por outra transação. Deste modo, a segunda transação está lendo dados que não foram confirmados ainda e podem ser alterados pela transação que atualiza a linha.

Por exemplo, um editor está fazendo mudanças em um documento eletrônico. Durante as mudanças, um segundo editor pega uma cópia do documento que inclui todas as mudanças feitas até o momento e distribui o documento para a audiência destinada.

O primeiro editor decide então que as mudanças feitas até o momento estão erradas, remove as edições e salva o documento. O documento distribuído contém edições que já não existem e que deveriam ser tratadas como se nunca tivessem existido.

Esse problema poderia ser evitado se ninguém pudesse ler o documento alterado até que o primeiro editor salvasse a versão final com as modificações e confirmasse a transação;

\3. **Análise inconsistente (leitura não-repetível)**: Ocorre análise inconsistente quando uma segunda transação acessa a mesma linha várias vezes e lê dados diferentes a cada vez.

A análise inconsistente é semelhante à dependência não confirmada, no sentido em que outra transação está alterando os dados que uma segunda transação está lendo.

No entanto, na análise inconsistente os dados lidos pela segunda transação foram confirmados pela transação que fez as alterações. Além disso, a análise inconsistente envolve leituras múltiplas (duas ou mais) da mesma fila, e a cada vez as informações são alteradas por outra transação; daí a denominação leitura não-repetível.

Por exemplo, um editor lê o mesmo documento duas vezes, mas entre cada leitura o escritor reescreve o documento. Quando o editor lê o documento pela segunda vez, este já foi alterado. A primeira leitura não era repetível.

Esse problema poderia ser evitado se o escritor não pudesse alterar o documento até que o editor tivesse terminado de lê-lo pela última vez;

\4. **Leituras fantasma**: Leituras fantasmas acontecem quando uma ação de inserção ou exclusão é executada em uma linha que pertence a um intervalo de linhas que estão sendo lidas por uma transação.

A primeira leitura do intervalo de linhas da transação mostra um registro que não existirá quando for feita a segunda leitura, porque esse registro foi excluído por uma transação diferente.

De maneira semelhante, a segunda leitura da transação mostra um registro que não existia na primeira leitura, porque foi realizada uma inserção por uma transação diferente.

Por exemplo, um editor faz alterações em um documento enviado por um escritor, mas quando as alterações são incorporadas na cópia mestra do documento pelo departamento de produção, descobre-se que material novo não editado foi acrescentado ao documento pelo autor.

De modo semelhante à situação de leitura não-repetível, esse problema poderia ser evitado se ninguém pudesse acrescentar material novo ao documento até o editor e o departamento de produção terminarem de trabalhar com o documento original.







**Nota: Stored procedure SP_LOCK**



A STORED PROCEDURE SP_LOCK serve de ferramenta de análise de LOCKS para tomada de decisão na estratégia de gerenciamento de bloqueio. É a SP responsável por dar as informações de todos os LOCKS que estão acontecendo no momento, gerando um relatório (RESULT SET) contendo as seguintes colunas:

**Spid**: Informa o código identificador da sessão Mecanismo de Banco de Dados para o processo solicitando o bloqueio;

**Dbid**: Informa o código identificador do banco de dados no qual o bloqueio é mantido;

**ObjId**: Informa o código identificador do objeto no qual o bloqueio é mantido;

**IndId**: Informa o código identificador do índice no qual o bloqueio é mantido;

**Type** (Tipo): Informa o tipo de bloqueio, dentre eles temos:

\1. RID: Bloqueio em uma única linha na tabela identificada por um identificador de linha (RID);

\2. KEY: Bloqueio dentro de um índice que protege um intervalo de chaves em transações serializáveis;

\3. PAG: Bloqueio em uma página de dados ou de índice;

\4. EXT: Bloqueio em uma extensão;

\5. TAB: Bloqueio em uma tabela inteira, inclusive todos os dados e índices;

\6. DB: Bloqueio em um banco de dados;

\7. FIL: Bloqueio em um arquivo de banco de dados;

\8. APP: Bloqueio em um recurso de aplicativo especificado;

\9. MD: Bloqueio em metadados ou informações do catálogo;

\10. HBT: Bloqueio em um índice heap ou árvore B;

\11. AU: Bloqueio em uma unidade de alocação.

**Resource** (Recurso): Informa o nome do recurso que está bloqueado;

**Mode** (Modo): Informa o modo do bloqueio, que pode ser:

1. NULL: Nenhum acesso concedido ao recurso. Funciona como espaço reservado;
2. SCH-S: Estabilidade do esquema. Assegura que um elemento de esquema, como uma tabela ou índice, não seja cancelado enquanto qualquer sessão mantém o bloqueio de estabilidade do esquema no elemento do esquema;
3. SCH-M: Modificação do esquema. Deve ser mantido por qualquer sessão que deseje alterar o esquema do recurso especificado. Assegura que nenhuma outra sessão esteja fazendo referência ao objeto indicado;
4. S: Compartilhado. A sessão base possui acesso compartilhado para o recurso;
5. U: Atualizar. Indica um bloqueio de atualização adquirido em recursos que podem ser atualizados eventualmente. É usado para evitar uma forma comum de deadlock que ocorre quando várias sessões bloqueiam recursos para uma atualização potencial em um momento posterior;
6. X: Exclusivo. A sessão base possui acesso exclusivo ao recurso;
7. IS: Tentativa compartilhada. Indica a intenção de colocar bloqueios S em algum recurso subordinado na hierarquia de bloqueio;
8. IU: Atualização da tentativa. Indica a intenção de colocar bloqueios U em algum recurso subordinado na hierarquia de bloqueio;
9. IX: Exclusivo da tentativa. Indica a intenção de colocar bloqueios X em algum recurso subordinado na hierarquia de bloqueio;
10. SIU: Atualização da tentativa compartilhada. Indica o acesso compartilhado a um recurso com a intenção de adquirir bloqueios de atualização em recursos subordinados na hierarquia de bloqueio;
11. SIX: Exclusivo da tentativa compartilhada. Indica o acesso compartilhado a um recurso com a intenção de adquirir bloqueios exclusivos em recursos subordinados na hierarquia de bloqueio;
12. UIX: Atualização exclusiva da tentativa. Indica a manutenção de um bloqueio de atualização de um recurso com a intenção de adquirir bloqueios exclusivos em recursos subordinados na hierarquia de bloqueio;
13. BU: Atualização em massa. Usado por operações em massa;
14. RangeS_S: Intervalo de chave compartilhada e bloqueio de recurso compartilhado. Indica exame de intervalo serializável;
15. RangeS_U: Intervalo de chave compartilhada e bloqueio de recurso de atualização. Indica exame de atualização serializável;
16. RangeI_N: Intervalo de chave de inserção e bloqueio de recurso nulo. Usado para testar intervalos antes de inserir uma chave nova em um índice;
17. RangeI_S: Bloqueio de conversão do intervalo de chave. Criado por uma sobreposição dos bloqueios RangeI_N e S;
18. RangeI_U: Bloqueio de conversão de intervalo de chave criado por uma sobreposição dos bloqueios RangeI_N e U;
19. RangeI_X: Bloqueio de conversão de intervalo de chave criado por uma sobreposição dos bloqueios RangeI_N e X;
20. RangeIX_S: Bloqueio de conversão de intervalo de chave criado por uma sobreposição dos bloqueios RangeI_N e RangeS-S;
21. RangeIX_U: Bloqueio de conversão de intervalo de chave criado por uma sobreposição dos bloqueios RangeI_N e RangeS-U;
22. RangeX_X: Bloqueio de intervalo de chave exclusivo e de recurso exclusivo. Este é um bloqueio de conversão usado na atualização de uma chave em um intervalo.

**Status**: É o estado em que o bloqueio se encontra, podendo ser:

1. CNVRT: O bloqueio está sendo convertido de outro modo, mas a conversão está bloqueada por outro processo que mantém um bloqueio com um modo conflitante;
2. GRANT: O bloqueio foi obtido;
3. WAIT: O bloqueio está bloqueado por outro processo que mantém um bloqueio com um modo conflitante.

Os recursos de modos de bloqueio que o mecanismo de banco de dados utiliza são:

\1. **Compartilhado (S)**: Usado para operações de leitura que não alteram ou atualizam dados, como uma instrução SELECT.

\2. **Atualização (U)**: Usado em recursos que podem ser atualizados. Evita uma forma comum de DEADLOCK que ocorre quando várias sessões estão lendo, bloqueando e potencialmente atualizando recursos mais tarde.

\3. **Exclusivo (X)**: Usado para operações da modificação de dados, como INSERT, UPDATE ou DELETE. Assegura que várias atualizações não sejam realizadas no mesmo recurso e ao mesmo tempo.

\4. **Intencional**: Usado para estabelecer uma hierarquia de bloqueio. Os tipos de bloqueios intencionais são: Tentativa compartilhada (IS), exclusivo de tentativa (IX) e compartilhado com exclusivo de tentativa (SIX).

\5. **Esquema**: Usado quando uma operação dependente do esquema de uma tabela está executando. Os tipos de bloqueios de esquema são: modificação de esquema (SCH-M) e estabilidade de esquema (SCH-S).

\6. **Atualização em massa (BU)**: Usado quando for copiar dados em massa em uma tabela e a dica**TABLOCK**(Especifica que um bloqueio compartilhado será usado na tabela bloqueada até o término da instrução) estiver especificada.

\7. **Intervalo de chave**: Protege o intervalo de leitura de linhas lido por uma consulta ao usar o nível de isolamento da transação serializável. Assegura que outras transações não possam inserir linhas que se qualifiquem para consultas da transação serializável se estas forem executadas novamente.



### Diretrizes para reduzir Locking e Bloqueios

Embora locking e bloqueios abranjam funcionalidades essenciais do SQL Server e forneçam a implementação da propriedade ACID (Atomicidade, Consistência, Isolamento e Durabilidade), você deve minimizar qualquer impacto negativo que possa ocorrer no desempenho do seu banco.

Você deve planejar várias estratégias que gerenciem os LOCKS do seu banco, fazendo isso desde o início do processo de desenvolvimento. Deste modo, você perceberá que é muito mais fácil otimizar os LOCKS do banco se considerar realizá-lo logo no início do ciclo de vida da aplicação do que quando a mesma já estiver com bastante tempo em produção.

Vários tipos de LOCKS são mantidos durante uma transação – dependendo do nível de isolamento escolhido. Se for preciso utilizar um alto nível de isolamento, garanta que as transações sejam de rápida execução. Quão logo a transação termine, mais cedo os LOCKS serão liberados e outras transações estarão aptas para acessar os dados.

Tente evitar os cursores porque tipicamente são lentos devido a sua natureza iterativa, e sempre que possível efetue a refatoração deles. Alguns tipos de cursores irão bloquear os dados durante todo o percurso do seu processamento, podendo causar bloqueios extensivos.

O SQL Server tentará bloquear na menor granularidade possível e atualizará somente um LOCK de grande granularidade se muitos dados estiverem sendo lidos ou se não existirem índices disponíveis que atendam a menor granularidade. Por exemplo, atualizando um LOCK de grande granularidade ocorrerá um alto custo de concorrência, pois, ao bloquear uma tabela inteira irá restringir o acesso a qualquer parte da tabela por outras transações.

Garanta que suas chaves de índices sejam altamente seletivas **e que suas queries** sejam escritas de maneira que utilizem os índices criados para que não façam TABLE SCANS. Dessa forma será garantido que o maior nível possível de granularidade seja usado.

Quanto mais for utilizado o maior nível de isolamento para uma transação, mais LOCKS serão alocados pelo SQL Server. Enquanto que no menor nível de isolamento o SQL Server não compartilha nenhum LOCK que faça com que o SGBD leia dados que não tenham sido confirmados (feito commit).

A maioria das aplicações não precisa utilizar o nível mais alto de isolamento. Elas podem tranquilamente utilizar o nível default (*read committed*), que garante um bom equilíbrio entre os níveis de baixa e alta restrição.

Mantenha um número baixo de TRIGGERS porque elas são disparadas depois de uma operação de manipulação de dados, mas antes que a transação seja confirmada.

Por este motivo, TRIGGERs alongam a duração das transações e causam maior incidência de bloqueios. Se for necessário fazer uso de TRIGGERs, garanta que elas sejam o mais eficiente possível, e evite utilizar códigos procedurais ou construções de loops dentro delas.

### Mandamentos

Finalizando o estudo, seguem alguns mandamentos a serem obedecidos na hora do desenvolvimento de uma query que ajudarão na performance geral do sistema:

1. Não utilizarás HINTS;
2. Não mexerás no mecanismo de LOCK;
3. Exigirás uma política de desenvolvimento para todos seguirem;
4. Deverás mexer no nível de isolamento de transação;
5. Sempre utilizarás FULL QUALIFIED NAME. Use FROM db_banco_teste.dbo.cidade ao invés de from cidade;
6. Evitarás o famoso SELECT asterisco, e colocarás somente o nome das colunas que desejares que o banco retorne;
7. Terás cautela com SUBQUERIES. Procurarás sempre que possível trocá-las por Joins. A cláusula EXISTS com SUBQUERIES será uma exceção;
8. Raciocinarás sempre em trabalhar com conjuntos de dados e nunca unitariamente, isto é, pensarás em conjuntos de dados e não em registros;
9. Deverás ter uma PRIMARY KEY (preferencialmente, esta sendo um índice clusterizado, pois toda tabela precisará ter no mínimo um índice destes) em todas as tuas tabelas;
10. Deverás ter uma quantidade apropriada de índices não clusterizados em todas as tabelas. Estes deverão ser criados nas colunas de uma tabela devido à necessidade de uma QUERY que esteja sendo colocada em produção;
11. Adotarás a seguinte ordem de prioridade quando qualquer índice for criado:

a. Cláusula WHERE;

b. Cláusula JOIN;

c. Cláusula ORDER BY;

d. Cláusula SELECT.

1. Removerás qualquer JOIN desnecessário das tuas consultas;
2. Evitarás o uso de VIEWS (geralmente são lentas e reduzem a performance);
3. Verificarás se o HD do servidor de banco de dados possui pelo menos 30% de espaço livre. Isto garante um pouco de performance.

##### Conclusão

Como dito em todo o artigo, a atividade de otimização de queries é uma arte. Portanto, são vários os sintomas e indicadores que devem ser analisados antes de tomar qualquer decisão.

Uma equipe deve ser unida para resolver o problema de lentidão de um sistema em todas as etapas, não interessando a sua área de trabalho, como: desenvolvimento, banco ou redes. Você deve acompanhar a solução até o final, mesmo que a sua área não tenha culpa do ocorrido.

O principal caminho a ser seguido para descobrir problemas de lentidão é utilizar o gráfico da pirâmide de otimização (apresentado em artigo publicado na edição 84 da SQL Magazine), mas isto, se todas as possibilidades físicas de rede e máquina que possam estar causando problemas já estiverem sido descartadas.