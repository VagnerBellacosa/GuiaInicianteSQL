# Oracle Database: Entendendo o conceito de Join e Outer Join

## Veja nesse artigo como trabalhar com join e outer join em um banco de dados para melhorar as consultas.

**Por que eu devo ler este artigo:**Esse artigo abordará algumas das melhores práticas de acesso aos dados em um ambiente de banco de dados. Isso será visto contrapondo algumas práticas obsoletas e problemáticas adotadas em ambientes coorporativos.



A discussão desse tema será útil, por exemplo, em situações onde o analista de sistemas se depara com a degradação de desempenho de certas rotinas havendo a necessidade de interagir com um profissional em administração de banco de dados para analisar e otimizar a rotina em questão.

Da mesma forma, será útil em projetos de desenvolvimento e implantação de sistemas de informação onde a estrutura de banco de dados poderá ser modelada respeitando as melhores práticas assim como preparando-a para o crescimento inevitável dos dados de forma ordenada.

Ver mais

[Voltar](https://www.devmedia.com.br/oracle/plsql)Suporte ao alunoAnotarMarcar como concluídoCódigo fonte

[Artigos](https://www.devmedia.com.br/artigos/)[Banco de Dados](https://www.devmedia.com.br/artigos/banco-de-dados)Oracle Database: Entendendo o conceito de Join e Outer Join





Com o aumento da complexidade dos sistemas de informação que gerenciam informações dentro de uma organização, um dos fatores de sucesso de um sistema é justamente a capacidade de recuperar e atualizar as informações de forma praticamente instantânea.

Uma modelagem de dados ruim é algo muito prejudicial ao desempenho e manutenção de um SGBD. Um dos exemplos mais clássicos é encontrar uma consulta ou uma visão de dados contemplando a existência de um right outer, left outer ou full outer join.

Os problemas de desempenho em uma aplicação que insere, atualiza e consulta informações em base de dados, geralmente se dão na escala abaixo:

· 60% são instruções SQL (consultas mal escritas);

· 20% referentes à arquitetura e modelo de dados;

· 18% referentes a ajustes de banco de dados;

· 2% associadas ao gerenciamento de do sistema operacional.

Mas, antes de falar de como essas situações atrapalham a vida de um DBA, vamos detalhar os conceitos de *join* e *outer join* em um banco de dados Oracle.

O *Join* ou *Inner Join* é uma consulta que combina registros de duas ou mais tabelas. A condição de junção compara duas colunas de diferentes tabelas, combinando pares de registros em cada tabela, sendo avaliada como verdadeiro se a combinação for encontrada em ambas as tabelas. A **Figura 1** representa este conceito enquanto que a **Listagem 1** indica como ele pode ser implementado.



![img](https://arquivo.devmedia.com.br/REVISTAS/sql/imagens/125/7/1.png)

**Figura 1.** Representação do join.

**Listagem 1.** Implementação do join.

```
select
a.id,
b.data
from
Tabela_A a,
Tabela_B b,
where
a.id = b.id

select
a.id,
b.data
from
Tabela_A a INNER OUTER JOIN Tabela_B b
on (a.id = b.id)
```



Já o *Outer Join* estende o resultado do *inner join* podendo retornar todos os registros de ambas as tabelas mesmo se a combinação for avaliada como falso, ou seja, combinando registros mesmo com desigualdades. Ele é subdividido em *right outer join*, *left outer join* e *full outer join*, como vemos abaixo.

O *Right outer join* foi pensado em uma junção entre uma tabela A e uma tabela B de forma a retornar todos os registros da tabela B do universo de dados em questão. Sendo assim, todos os registros do universo de dados em questão da tabela B serão selecionados e quando a combinação da junção for falso, valores classificados como *null* serão listados para os campos selecionados da tabela A. A **Figura 2** representa este conceito enquanto que a **Listagem 2** indica como ele pode ser implementado.

![img](https://arquivo.devmedia.com.br/REVISTAS/sql/imagens/125/7/2.png)

**Figura 2.** Representação do right outer join.

**Listagem 2.** Implementação do right outer join.

```
select
a.id,
b.data
from
Tabela_A a,
Tabela_B b,
where
a.id = b.id(+) ...
```