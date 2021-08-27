# Merge join cartesian: Uma possível causa de mau desempenho

## Conheça uma das diversas maneiras de se evitar que consultas SQL em bancos de dados Oracle utilizem planos cartesianos, o que prejudica o desempenho de instruções SQL.



Por que eu devo ler este artigo:Consultas SQL que possuam em seus critérios seletivos busca por dados referentes a colunas pertencentes a mais de uma tabela ou views (inclusive de dicionário) em situações em que não exista qualquer relacionamento entre as tabelas em questão, uma das formas mais comuns adotada pelo otimizador de consultas do gerenciador de banco de dados é executá-la por meio de uma junção cartesiana no plano de execução. Isso significa que, para cada linha da **Tabela 1**, todas as linhas da **Tabela 2** serão verificadas, e se necessário, retornadas ao processo solicitante da consulta. Esse cenário degrada significativamente o desempenho da consulta, criando um cenário propício para prováveis contenções. Este artigo abordará uma das diversas maneiras de se evitar que consultas SQL em bancos de dados Oracle utilizem planos cartesianos como parte de seus planos de execução, o que prejudica a expectativa de desempenho – eleva o tempo total de execução – de instruções SQL. Para isso, serão conceituados alguns pontos relacionados a esse problema como tipos e métodos de junções, além de componentes do otimizador de consultas, estatísticas e parâmetros de inicialização da instância.

Ver mais

[Voltar](https://www.devmedia.com.br/oracle/oracle-para-dbas)Suporte ao alunoAnotarMarcar como concluído

[Artigos](https://www.devmedia.com.br/artigos/)[Banco de Dados](https://www.devmedia.com.br/artigos/banco-de-dados)Merge join cartesian: Uma possível causa de mau desempenho

Uma das melhores formas para que se adquira confiança no bom desempenho de bancos de dados pode ser a utilização correta de configurações disponíveis no gerenciador, de forma a evitar situações onde a ausência de relacionamento entre tabelas possa criar junções cartesianas nos planos de execução utilizados pelo otimizador de consultas.

O plano (ou produto) cartesiano, do ponto de vista técnico, é utilizado como alternativa de execução para consultas SQL em um banco de dados que não possuam restrições seletivas suficientes, tanto em sua cláusula WHERE, quanto na cláusula FROM, e que efetuem buscas por informações em dois ou mais objetos fontes de dados (tabelas ou views) que não possuam relacionamento entre si. Isso cria o cenário para a formação de um produto cartesiano entre todas as linhas, de todos os objetos ou tabelas envolvidas na consulta em questão. O resultado da consulta sempre será o produto (multiplicação simples) entre a quantidade de linhas de cada tabela mencionada na cláusula SQL.

Para que melhor se compreenda um produto ou plano cartesiano, basta supor uma consulta SQL qualquer que consulte dados em duas tabelas: uma tabela com 1000 registros, juntamente com uma consulta, ou subconsulta, a uma segunda tabela que possua 10000 registros. O retorno seriam absurdos 10 milhões de linhas. Alto custo de processamento (utilização de recursos do servidor), tempo de processamento, utilização excessiva de recursos de comunicação, agilidade, desempenho, entre muitos outros, são fatores extremamente sensíveis a esse tipo de situação, que pode ser a origem de lentidões generalizadas, interrupções de serviço, indisponibilidades de sistemas e demais incidentes relacionados.

Neste artigo, veremos como evitar a ocorrência de produtos cartesianos em consultas utilizando dois parâmetros de inicialização da instância: _optimizer_cartesian_enabled e _optimizer_mjc_enabled. Esses parâmetros desabilitam a utilização de planos cartesianos, impondo ao gerenciador a premissa de utilização de um plano de execução diferente, fato que por consequência, altera a expectativa de desempenho do processo em questão.

No código a seguir temos um exemplo de consulta SQL, sem restrições seletivas nas cláusulas WHERE ou FROM, executada em um banco Oracle:

```
SQL> select a.language_id, b.document_id
    2  from fnd_languages a, fnd_documents_tl b;
```

Parte da saída da consulta SQL anterior pode ser visualizada na **Tabela 1**.

**Tabela 1.** Resultado da consulta SQL

| *LANGUAGE_ID* | *DOCUMENT_ID* |
| ------------- | ------------- |
| *8*           | *13649575*    |
| *8*           | *13649575*    |
| *8*           | *13649576*    |
| *8*           | ...           |