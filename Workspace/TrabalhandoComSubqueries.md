# Trabalhando com subqueries

## Aprenda mais sobre subqueries! Uma ferramenta que hoje em dia não pode faltar para o desenvolvedor, pois existem certas consultas onde ela é a única opção para o resultado correto.



**Uma Subquery (também conhecida como SUBCONSULTA ou SUBSELECT) é uma instrução do tipo SELECT dentro de outra instrução SQL**. Desta forma, se torna possível efetuar consultas que de outra forma seriam extremamente complicadas ou impossíveis de serem feitas de outra forma.

Abaixo segue um exemplo de uma SUBQUERY:

```
SELECT
    *
FROM
    tabela1 AS T
WHERE
    coluna1 IN
    (
        SELECT
            coluna2
        FROM
            tabela2 AS T2
        WHERE
            T.id = T2.id
    )
```

No exemplo acima temos uma primeira instrução SELECT realizando um filtro através da cláusula IN dentro de outro SELECT, ou seja, uma consulta dentro do resultado de outra consulta.

### Como utilizar?

Existem algumas formas de se utilizar subqueries. Neste artigo abordaremos os seguintes meios:

- **Subquery** como uma nova coluna da consulta (SELECT AS FIELD).
- **Subquery** como filtro de uma consulta (utilizando IN, EXISTS ou operadores de comparação).
- **Subquery** como fonte de dados de uma consulta principal (SELECT FROM SELECT)

### Estrutura do banco de dados de exemplo

Para ilustração dos exemplos a seguir, considere a estrutura de tabelas abaixo (**Figura 1**), onde temos a tabela projetos (**Tabela 1**), a tabela comentarios (**Tabela 2**), a tabela usuarios (**Tabela 3**), a tabela likes_por_projeto (**Tabela 4**) e a tabela likes_por_comentarios (**Tabela 5**).

![Estrutura do Banco](https://arquivo.devmedia.com.br/pablo/pacotes/17mEi3MAawuTIcHqmTWPIsGXRiUI4PNVFviJu5oP.png)**Figura 1**. Modelagem do banco

| id   | titulo           | data       |
| ---- | ---------------- | ---------- |
| 1    | Aplicação C#     | 2018-04-01 |
| 2    | Aplicação Ionic  | 2018-05-07 |
| 3    | Aplicação Python | 2018-08-05 |

**Tabela 1.** Estrutura da tabela projetos

| id   | comentario                              | id_projeto | id_usuario |
| ---- | --------------------------------------- | ---------: | ---------- |
| 1    | A microsoft acertou com essa linguagem! |          1 | 1          |
| 2    | Parabéns pelo projet! bem legal!        |          1 | 3          |
| 3    | Super interessante! Fácil e rapido!     |          2 | 4          |
| 4    | Cara, que simples fazer um app assim!   |          2 | 1          |
| 5    | Linguagem muito diferente.              |          3 | 3          |
| 6    | Adorei aprender Python! Parabéns!       |          3 | 2          |
| 7    | Muito maneiro esse framework!           |          2 | 2          |

**Tabela 2.** Estrutura da tabela comentarios

| id   | nome             | email                     | senha       |
| ---- | ---------------- | ------------------------- | ----------- |
| 1    | Bruna Luiza      | bruninha@gmail.com        | abc123.     |
| 2    | Thiago Braga     | thiagobraga_1@hotmail.com | pena093     |
| 3    | Osvaldo Justino  | osvaltino@yahoo.com.br    | osvaldit1_s |
| 4    | Gabriel Fernando | gabriel_fnd@gmail.com     | gabss34     |

**Tabela 3.** Estrutura da tabela usuarios

| id_projeto | id_usuario |
| ---------- | ---------- |
| 1          | 1          |
| 1          | 3          |
| 2          | 1          |
| 2          | 2          |
| 2          | 3          |
| 2          | 4          |
| 3          | 2          |

**Tabela 4.** Estrutura da tabela likes_por_projeto

| id_comentario | id_usuario |
| ------------- | ---------- |
| 7             | 1          |
| 7             | 2          |
| 7             | 4          |

**Tabela 5.** Estrutura da tabela likes_por_comentarios

### Subquery como uma nova coluna da consulta

Uma das formas possíveis de se *realizar uma subquery* é fazendo com que o resultado de outra consulta seja uma coluna dentro da sua consulta principal.

Como exemplo iremos buscar o título de todos os projetos cadastrados e adicionar uma coluna com a quantidade de comentários existe em cada projeto, realizando assim uma consulta principal na tabela de projetos e uma subconsulta na tabela de comentarios, que irá gerar uma nova coluna.

Veja o resultado esperado na **Tabela 6**.

| titulo           | Quantidade_Comentarios |
| ---------------- | ---------------------- |
| Aplicação C#     | 2                      |
| Aplicação Ionic  | 3                      |
| Aplicação Python | 2                      |

**Tabela 6.** Resultado da query

Na resultado acima existem duas colunas: a coluna titulo e a coluna Quantidade_Comentarios. Essa tabela foi obtida através da consulta SQL abaixo:

```
SELECT
            P.titulo,
            (SELECT
                COUNT(C.id_projeto)
            FROM
                comentarios C
            WHERE
                C.id_projeto = P.id ) AS Quantidade_Comentarios
        FROM
            projetos P
        GROUP BY
            P.id
```

Observe na query acima que a consulta principal acima é feita na tabela projetos, porém, na seleção das colunas que virão no resultado existe uma outra consulta, essa na tabela comentarios, responsável por trazer o total de ocorrências do ID do projet específico na tabela de comentários. Essa coluna foi nomeada de Quantidade_Comentarios.

Podemos utilizar o trecho de código abaixo para adicionar mais uma coluna a esta consulta. Adicionaremos, por exemplo, o valor total de likes recebidos por projeto:

```
SELECT
    P.titulo,
    (SELECT
        COUNT(C.id_projeto)
    FROM
        comentarios C
    WHERE
        C.id_projeto = P.id ) AS Quantidade_Comentarios,
    (SELECT
        COUNT(LP.id_projeto)
    FROM
        likes_por_projeto LP
    WHERE
        LP.id_projeto = P.id ) AS Quantidade_Likes
FROM
    projetos P
GROUP BY
    P.id
```

Veja o resultado da query acima na **Tabela 7**.

| titulo           | Quantidade_Comentarios | Quantidade_likes |
| ---------------- | ---------------------- | ---------------- |
| Aplicação C#     | 2                      | 2                |
| Apliocação Ionic | 3                      | 4                |
| Aplicação Python | 2                      | 1                |

**Tabela 7.** Resultado da query

Observe que no resultado acima existem três colunas, sendo que apenas a coluna titulo faz parte da tabela projetos. As colunas Quantidade_Comentarios e Quantidade_likes são providas pelas duas subqueries utilizadas em tabelas diferentes, comentarios e likes_por_projeto, respectivamente.

Sempre após a criação de uma subquery como nova coluna será necessário definir um nome para esta coluna, através da palavra reservada AS. Uma subquery como nova coluna deve retornar apenas uma única coluna com um único valor.

### Subquery como filtro de uma nova consulta

Outro exemplo da utilização de subqueries é fazendo filtros no resultado de outras consultas. Para esse modelo podemos utilizar as cláusulas IN, EXISTS ou operadores de comparação, como =, >=, <=, dentre outros.

Para exemplificar iremos buscar todos os projetos que possuam algum comentário, ou seja, uma consulta principal na tabela projetos, e filtrar o resultado com base no resultado da subconsulta na tabela de comentarios.

Veja ao resultado esperado na **Tabela 8**.

| id   | titulo           | data       |
| ---- | ---------------- | ---------- |
| 1    | Aplicação C#     | 2018-04-01 |
| 2    | Aplicação Ionic  | 2018-05-07 |
| 3    | Aplicação Python | 2018-08-05 |

**Tabela 8.** Resultado da query

Observe no resultado acima que vieram apenas informações da tabela projetos, mas que foram filtradas com base em uma pesquisa pelo ID do projeto na tabela de comentários. Cada projeto listado neste resultado é um projeto que possui algum comentário. Essa informação foi obtida através da query abaixo:

```
SELECT
    P.id,
    P.titulo,
    P.data
FROM
    projetos P
WHERE
    P.id IN
    (
        SELECT
            C.id_projeto
        FROM
            comentarios C
        WHERE
            P.id = C.id_projeto
    );
```

Observe que na query acima a consulta principal é feita na tabela projetos, mas que em seguida utilizamos a cláusula WHERE para realizar um filtro na seleção, definindo através da cláusula IN que o valor da coluna ID deve estar incluso no resultado da subconsulta realizada, que no caso é feita na tabela de comentários.

Uma subquery utilizada como filtro de uma consulta principal pode retornar N valores, porém, apenas uma única coluna.

Podemos utilizar também a cláusula EXISTS pra utilizar uma subquery como filtro. Para exemplificar podemos realizar a mesma consulta anterior, buscar todos os projetos que possuam algum comentário:

```
SELECT
            P.id,
            P.titulo,
            P.data
        FROM
            projetos P
        WHERE
            EXISTS
            (
                SELECT
                    C.id_projeto
                FROM
                    comentarios C
                WHERE
                    P.id = C.id_projeto
            );
```

**Na query acima continuamos com o mesmo resultado que a query anterior, mudando apenas a forma como é feita a subquery**. No caso da cláusula EXISTS a query principal realiza uma verificação se o resultado da segunda query conseguiu encontrar algum valor, e em caso positivo, retorna esse item, ou seja, ao encontrar um comentário para algum projeto, os dados solicitados na consulta principal a respeito desse projeto serão exibidos.

Outro exemplo do mesmo modelo de subquery seria utilizando operadores lógicos. Buscaremos agora o titulo e a data do último projeto que recebeu likes, ou seja, uma consulta principal na tabela de projetos com um filtro na tabela de likes:

```
SELECT
            P.titulo,
            P.data
        FROM
            projetos P
        WHERE
            P.id = (SELECT
            MAX(LP.id_projeto)
        FROM
            likes_por_projeto LP);
```

Observe que na query acima a consulta principal é feita na tabela projetos e o filtro realizado através da cláusula WHERE é que o ID do projeto seja IGUAL ao valor retornado pela subquery, que busca pelo MAIOR valor de ID do projeto na tabela likes_por_projeto. Veja o resultado obtido na **Tabela 9**.

| titulo           | data       |
| ---------------- | ---------- |
| Aplicação Python | 2018-08-05 |

**Tabela 9.** Resultado da query

### Subquery como fonte de dados de uma consulta principal

Este outro formato faz com que o **resultado de uma subquery** possa ser utilizado como tabela fonte de dados de uma consulta principal.

Para exemplificar esse modelo, realizaremos primeiro a query que servirá como fonte de dados para a consulta principal:

```
SELECT
    P.id,
    P.titulo,
    (SELECT
        COUNT(C.id_projeto)
    FROM
        comentarios C
    WHERE
        C.id_projeto = P.id ) AS Quantidade_Comentarios
    FROM
        projetos P
```

Veja abaixo o resultado da query acima na **Tabela 10**.

| titulo           | Quantidade_Comentarios |
| ---------------- | ---------------------- |
| Aplicação C#     | 2                      |
| Apliocação Ionic | 3                      |
| Aplicação Python | 2                      |

**Tabela 10.** Resultado da query

Com base no resultado acima, iremos selecionar apenas o projeto que teve a quantidade de comentarios maior de 2, dessa forma, utilizaremos a query acima como fonte de dados.

```
SELECT
    F.titulo,
    F.Quantidade_Comentarios
FROM
    (SELECT
        P.id,
        P.titulo,
        (SELECT
            COUNT(C.id_projeto)
        FROM
            comentarios C
        WHERE
            C.id_projeto = P.id ) AS Quantidade_Comentarios
FROM
    projetos P
) as F
WHERE
    F.Quantidade_Comentarios > 2
```

Observe que na query acima, a consulta principal solicita através do FROM as colunas titulo, Quantidade_Comentarios da fonte de dados baseada em uma outra consulta, e por fim, realiza um filtro no resultado através da cláusula WHERE para buscar somente aqueles projetos com a quantidade de comentarios maior que 2.

Sempre após a criação de uma subquery como fonte de dados de uma consulta principal será necessário definir um nome para esta fonte de dados, através da palavra reservada AS.

Para mais informações, temos aqui na DevMedia um curso exclusivo sobre este tema! [Avançando com Subqueries](https://www.devmedia.com.br/curso/avancando-com-subqueries/2231)

[Saiba maisVeja a Série **SQL nível Jedi: Subqueries**](https://www.devmedia.com.br/sql-queries/)

