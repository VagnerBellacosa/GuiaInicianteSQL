# Entendendo a instrução JOIN

## Este artigo apresenta o uso de instruções SELECT que tragam o resultado de duas ou mais tabelas, isto é, faça joins entre estas tabelas. Serão apresentados uma série de exemplos para ilustrar e tornar o seu entendimento mais fácil.

O objetivo desse artigo é apresentar a instrução SELECT e seus detalhes de uso, principais características, algumas diferenças na sua implementação entre os diferentes fabricantes de SGBD, assim como suas melhores práticas e, desde o início, como escrevê-la de forma que tenha sempre boa performance.

Ver mais

[Artigos](https://www.devmedia.com.br/artigos/)[Banco de Dados](https://www.devmedia.com.br/artigos/banco-de-dados)Entendendo a instrução JOIN

Nosso foco principal será a preparação de [instruções SELECT](https://www.devmedia.com.br/sql-select-entendendo-a-instrucao-select/32250) que tragam o resultado de duas ou mais tabelas, isto é, **façam joins entre estas tabelas e retorne seus dados num único comando**. Serão apresentados uma série de exemplos para ilustrar e tornar o seu entendimento mais fácil.

------

##### Guia do artigo:

- [Modelo de dados](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#1)
- [Teoria de conjuntos](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#2)
- [SELECT FROM](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#3)
- [Inner Joins e Outer Joins](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#4)
- [Sintaxes](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#5)
- [Um exemplo de join de várias tabelas](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#6)
- [Por que o Join?](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#7)
- [Outras sintaxes de Outer Joins](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#8)
- [O operador UNION](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#9)
- [O operador INTERSECT](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#10)
- [O operador MINUS](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#11)
- [Auto Join](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#12)
- [Cross Join](https://www.devmedia.com.br/entendendo-a-instrucao-join/32936#13)

------

Com esses conceitos você descobrirá qual tipo de join se adapta corretamente ao seu problema, como escrevê-lo da forma correta, e também que pode escrever muito menos linhas de código na sua aplicação porque uma instrução SELECT poderá fazer grande parte desse trabalho para você.

Portanto, se você é um desenvolvedor, você já utilizou ou utilizará essa instrução. Assim, nada melhor do que conhecer seus detalhes e como utilizá-la de maneira que torne sua aplicação o mais eficiente possível no acesso aos seus dados.

Em um artigo publicado na edição 129, fizemos uma introdução à instrução SELECT da linguagem SQL. Nele apresentamos sua forma básica de uso de tal modo que você já conseguisse escrever as primeiras instruções, recuperando dados do seu banco de dados, utilizando funções muito úteis no seu dia a dia.

Este artigo avançará mais profundamente nessa mesma instrução SELECT. Vamos colocar seu conhecimento da instrução em seu nível intermediário. Suficiente para utilizá-la em 90% das situações enfrentadas diariamente por um desenvolvedor.

Novamente, utilizaremos as [melhores práticas](https://www.devmedia.com.br/boas-praticas-de-programacao/31163) como nossa orientação primordial. Como melhores práticas você poderá entender: instruções simples, escritas de forma clara e correta, e apresentando a melhor performance possível.

O foco nesse artigo será como apresentar os dados de duas ou mais tabelas em uma única instrução, isto é, **como montar joins**. Vamos apresentar e conceituar os diferentes tipos de joins e suas diferentes formas de escrevê-los.

### Modelo de dados de exemplos

O assunto central deste artigo são as várias formas de join, ou combinação de várias tabelas em uma única instrução. Para se combinar tabelas, é primordial conhecer como estas tabelas foram modeladas e se relacionam.

Não conseguiríamos escrever um join sem saber quais são as colunas de duas tabelas que definem o seu relacionamento. Portanto, para termos sucesso e clareza em nossos exemplos, vamos apresentar o modelo de dados que será utilizado para desenvolvermos as instruções SELECT ao longo desse artigo.

Na **Figura 1** é possível ver o esquema simplificado dos relacionamentos entre as tabelas. Nosso objetivo aqui não é falar sobre este tipo de esquema, chamado de Modelo de Entidades e Relacionamentos, que tem muito mais símbolos do que os apresentados aqui. Por isso, apresentaremos rapidamente o que ele está descrevendo.

Nesse conjunto de tabelas, temos a listagem dos materiais (tabela Materiais) utilizados por uma empresa fictícia para a qual vamos montar nossos exemplos. Cada um desses materiais é comprado de uma empresa fornecedora (tabela Fornecedores).

Temos também os pedidos de compra que foram feitos para cada fornecedor (tabela Pedidos), com seus respectivos itens (tabela “Itens x Pedidos”), isto é, quais materiais foram solicitados nos pedidos. A última tabela é a tabela de unidades. Nela estão as definições das unidades de medida utilizadas para contar ou medir os itens.

As setas que ligam uma tabela, ou entidade, as outras representam os relacionamentos que existem entre elas. O sentido da seta indica qual tabela é pai e qual é filha nesse relacionamento, estando a filha do lado da seta.

Para ficar mais claro, existe uma seta entre a tabela de Fornecedores e Materiais, apontando para tabela Materiais. Isto indica que existe um relacionamento entre as duas tabelas e que a tabela Materiais é filha da tabela Fornecedores.

Assim, a tabela Materiais deverá possuir uma coluna que identifique qual a empresa fornecedora é a que oferece aquele material. Na lista de atributos, ou colunas, das tabelas apresentadas, é possível notar que existe uma coluna cod_fornecedor na tabela Materiais e este é exatamente o propósito dela, indicar quem é o seu fornecedor.

A intenção deste pequeno modelo não é sermos exaustivos em todas as possibilidades de negócio que poderiam ser definidas nesse conjunto de tabelas, mas apenas utilizá-las como exemplo. Só para exemplificar uma regra de negócio que não é atendida por este modelo, um Material só pode ser fornecido por uma empresa fornecedora. Não existe aqui como armazenar ou representar um material oferecido por mais de um fornecedor. A seguir são apresentados os atributos de cada tabela:

- Fornecedores: cod_fornecedor, nome, cidade_sede, grupo_cod_fornecedor;
- Materiais: cod_material, cod_fornecedor, nome, descricao, quant_estoque, quant_estoque_min, cod_unidade;
- Pedidos: num_pedido, cod_fornecedor, data_pedido, data_recebimento, quant_itens, valor_total;
- Itens_Pedidos: num_pedido, cod_material, quant_pedida, valor_unitario;
- Unidades: cod_unidade, nome_unidade.

![Modelo de relacionamento das tabelas dos exemplos desse artigo](https://arquivo.devmedia.com.br/REVISTAS/sql/imagens/132/4/1.png)**Figura 1**. Modelo de relacionamento das tabelas dos exemplos desse artigo

Note que na lista de colunas de cada tabela uma ou mais colunas ou atributos estão destacadas em negrito. Elas são as colunas chamadas de chave primária de cada tabela, isto é, as colunas que identificarão unicamente cada linha de dados de cada tabela. A coluna destacada na tabela Fornecedores é a cod_fornecedor. Isto significa que cada fornecedor deverá ter um código único, representado nessa coluna, e esse código identificará apenas um fornecedor.

A coluna cod_fornecedor aparece também na tabela Materiais. Isto serve para identificarmos que aquele material é oferecido pelo fornecedor que aparecer com o código ao lado. A coluna cod_fornecedor, quando aparece na tabela Materiais para definir o relacionamento, é chamada de chave estrangeira. A **Listagem 1** apresenta os dados de exemplo desse conjunto de tabelas.

```
select * from fornecedores;
+-----------+-------------------------------------------+----------------+----------------------+
| cod_forne | nome                                      | cidade_sede    | grupo_cod_fornecedor |
+-----------+-------------------------------------------+----------------+----------------------+
| ABC       | ABC Materiais Eletricos                   | Vitoria        | NULL                 |
| XYZ       | XYZ Materiais de Escritorio               | Rio de Janeiro | HiX                  |
| Hidra     | Hidra Materiais Hidraulicos               | Sao Paulo      | HiX                  |
| HiX       | HidraX Materiais ElÈtricos e Hidraulicos  | Sao Paulo      | NULL                 |
+-----------+-------------------------------------------+----------------+----------------------+

select cod_material, cod_fornecedor, nome, descricao from materiais order by cod_material;
+--------------+----------------+----------------------------+---------------------------------+
| cod_material | cod_fornecedor | nome                       | descricao                       |
+--------------+----------------+----------------------------+---------------------------------+
|          123 | ABC            | Tomada eletrica 10A Nova   | Tomada eletrica 10A padrao novo |
|          234 | ABC            | Disjuntor 25A              | Disjuntor eletrico 25A          |
|          345 | XYZ            | Resma Papel A4             | Resma papel branco A4           |
|          456 | XYZ            | Toner Imp HR5522           | Toner impressora HR5522         |
|          678 | Hidra          | Cano PVC 1/2               | Cano PVC 1/2 pol                |
|          679 | Hidra          | Cano PVC 3/4               | Cano PVC 3/4 pol                |
|          680 | Hidra          | Medidor hidr 1/2           | Medidor hidraulico 1/2 pol      |
|          681 | Hidra          | Joelho 1/2                 | Conector Joelho 1/2 pol         |
|          682 | Hidra          | Junta 1/2                  | Cano PVC 1/2 pol                |
|         1234 | ABC            | Tomada eletrica 20A Nova   | Tomada eletrica 20A padrao novo |
|         2345 | XYZ            | Caneta Azul                | Caneta esferografica azul       |
|         4567 | XYZ            | Grapeador                  | Grampeador mesa pequeno         |
|         4568 | XYZ            | Caneta Marca Texto Amarela | Caneta Marca Texto Amarela      |
|         4569 | XYZ            | Lapis HB                   | Lapis Preto HB                  |
+--------------+----------------+----------------------------+---------------------------------+

select cod_material, quant_estoque, quant_estoque_min,
cod_unidade from materiais order by cod_material;
+--------------+---------------+-------------------+-------------+
| cod_material | quant_estoque | quant_estoque_min | cod_unidade |
+--------------+---------------+-------------------+-------------+
|          123 |            12 |                 5 | UN          |
|          234 |            10 |                 5 | UN          |
|          345 |            32 |                20 | CX12        |
|          456 |             6 |                10 | UN          |
|          678 |             6 |                10 | NULL        |
|          679 |             8 |                10 | NULL        |
|          680 |             3 |                 2 | NULL        |
|          681 |            18 |                15 | NULL        |
|          682 |             0 |                15 | NULL        |
|         1234 |             8 |                 5 | UN          |
|         2345 |            80 |               120 | UN          |
|         4567 |             5 |                 5 | UN          |
|         4568 |             6 |                15 | CX100       |
|         4569 |            15 |                25 | UN          |
+--------------+---------------+-------------------+-------------+

select * from pedidos;
+------------+----------------+-------------+------------------+-------------+-------------+
| num_pedido | cod_fornecedor | data_pedido | data_recebimento | quant_itens | valor_total |
+------------+----------------+-------------+------------------+-------------+-------------+
|        111 | XYZ            | 2015-02-25  | 2015-03-31       |         200 |       75.00 |
|        115 | Hidra          | 2014-02-10  | 2014-04-10       |          50 |       65.00 |
|        120 | XYZ            | 2015-03-01  | 2015-03-21       |         200 |       75.00 |
+------------+----------------+-------------+------------------+-------------+-------------+

select * from itens_pedidos;
+------------+--------------+--------------+----------------+
| num_pedido | cod_material | quant_pedida | valor_unitario |
+------------+--------------+--------------+----------------+
|        111 |         2345 |          100 |           0.50 |
|        111 |         4569 |          100 |           0.25 |
|        115 |          682 |           50 |           1.30 |
|        120 |         4567 |            5 |          76.00 |
+------------+--------------+--------------+----------------+

select * from unidades;
+-------------+------------------------+
| cod_unidade | nome                   |
+-------------+------------------------+
| UN          | Unidades               |
| KG          | Kilogramas             |
| LT          | Litros                 |
| CX12        | Caixa com 12 unidades  |
| CX100       | Caixa com 100 unidades |
+-------------+------------------------+
```

**Listagem 1**. Dados das tabelas de exemplo

### Teoria de conjuntos

Quando estamos falando do acesso aos dados de uma base de dados, eles podem serem chamados conjuntos, conjuntos de dados. Ao decorrer deste artigo vamos utilizar desta comparação para facilitar o entendimento dos exemplos.

A teoria dos conjuntos estuda coleções de elementos. Associando com bancos de dados, nossos elementos são os dados das tabelas. Utilizando nosso modelo de dados definido para este artigo, podemos identificar alguns exemplos de conjuntos de dados:

- O conjunto dos fornecedores que receberam pedido nos últimos seis meses;
- O conjunto dos materiais com o estoque zerado;
- O conjunto dos fornecedores que atenderam aos pedidos em até uma semana;

E, relembrando os conceitos combinação de conjuntos temos:

- União de conjuntos – O resultado da união de dois conjuntos A e B é um outro conjunto com todos os elementos de A e de B;
- Interseção de conjuntos – O resultado da interseção de dois conjuntos A e B são apenas os elementos que são coincidentes nos dois conjuntos, isto é, os elementos que existem em ambos simultaneamente;
- Diferença de conjuntos – O resultado da diferença do conjunto A menos o conjunto B são os elementos do conjunto A excluindo aqueles que também existem no conjunto B;
- Produto cartesiano de conjuntos – O resultado do produto cartesiano entre os conjuntos A e B são pares de elementos, um de cada conjunto, combinando todos os elementos de A com todos os elementos de B;
- Relação de pertinência – É quando um conjunto está contido em outro conjunto. O conjunto A está contido em um conjunto B quando todos os elementos de A também fazem parte do conjunto B. Em outras palavras, também podemos dizer que o conjunto A é um subconjunto do conjunto B.

### SELECT FROM duas ou mais tabelas

A primeira característica da instrução SELECT que gostaríamos de apresentar nesse artigo é o caso mais **simples de join**, ou como se obter informações de mais de uma tabela simultaneamente no mesmo comando. Ou, como unir ou juntar as informações de mais de uma tabela na mesma instrução.

A sintaxe da instrução SELECT com uso de joins é:

```
SELECT <lista de colunas>
FROM <nome de uma ou mais tabelas>
WHERE <lista de condições>
```

O principal item a ser destacado agora é que, na cláusula FROM, em vez de apresentarmos o nome de apenas uma tabela, podemos incluir uma lista de tabelas, separadas por vírgulas. Temos ainda a mesma lista de colunas após a palavra SELECT e os mesmos tipos de condição encontradas na [cláusula WHERE](https://www.devmedia.com.br/sql-clausula-where/37645).

Começando diretamente com um exemplo, se nós quiséssemos trazer a lista de todos os materiais junto ao nome dos seus fornecedores correspondentes, teríamos a instrução escrita da seguinte forma:

```
SELECT fornecedores.nome, materiais.nome
FROM fornecedores, materiais;
```

Além da lista das duas tabelas, fornecedores e materiais, na [cláusula FROM](https://www.devmedia.com.br/clausulas-inner-join-left-join-e-right-join-no-sql-server/18930#1), temos ainda uma outra característica na lista de colunas seguidas à palavra SELECT: cada coluna está prefixada com o nome da sua respectiva tabela. Isso só é necessário porque a coluna “nome” existe nas duas tabelas. Por esse motivo, temos que dizer para o SGBD de qual coluna, de qual tabela, estamos nos referenciando.

Tudo seria muito simples se pudéssemos parar um “join” por aqui, porém, o comando não pode ser escrito apenas dessa forma. Desse jeito, o SGBD não sabe como as duas tabelas se relacionam.

E sem dizermos como as tabelas Fornecedores e Materiais se relacionam, o SGBD vai interpretar que ele deve trazer para você todos os materiais para cada linha de fornecedor que ele encontrar.

Isto é, ele vai entender que o relacionamento existente é: todas as linhas da tabela de fornecedores se relacionam com todas as linhas da tabela de materiais e isso não é verdade. Na realidade, um material é oferecido por apenas um fornecedor e nós precisamos dizer isso ao SGBD.

E a forma de dizer isso é incluindo uma restrição na cláusula WHERE onde vamos dizer para o banco de dados que ele deve nos trazer apenas os materiais relacionados aos seus fornecedores correspondentes, ou de outra forma, ele deve apresentar todos os fornecedores e os materiais que cada um tem disponível para comercializar. Assim, nosso comando complementado com a cláusula WHERE, ficaria conforme a **Listagem 2**.

```
SELECT fornecedores.nome “Nome Fornecedor”, materiais.nome “Nome Material”
FROM fornecedores, materiais
WHERE fornecedores.cod_fornecedor = materiais.cod_fornecedor;
```

**Listagem 2**. Exemplo utilizando cláusula Where

Esta restrição diz que o banco só deve nos retornar os nomes dos fornecedores e materiais para as linhas onde o código do fornecedor seja igual em ambas as tabelas. O resultado da execução dessa instrução está representado na **Listagem 3**.

```
+--------------------------------+----------------------------------+
| Nome Fornecedor                 | Nome Material                   |
+--------------------------------+----------------------------------+
| ABC Materiais Eletricos         | Tomada eletrica 10A Nova        |
| ABC Materiais Eletricos         | Tomada eletrica 20A Nova        |
| ABC Materiais Eletricos         | Disjuntor 25A                   |
| XYZ Materiais de Escritorio   | Caneta Azul                       |
| XYZ Materiais de Escritorio   | Resma Papel A4                    |
| XYZ Materiais de Escritorio   | Toner Imp HR5522                  |
| XYZ Materiais de Escritorio   | Grapeador                         |
| XYZ Materiais de Escritorio   | Caneta Marca Texto Amarela        |
| XYZ Materiais de Escritorio   | Lapis HB                          |
| Hidra Materiais Hidraulicos   | Cano PVC 1/2                      |
| Hidra Materiais Hidraulicos   | Cano PVC 3/4                      |
| Hidra Materiais Hidraulicos   | Medidor hidr 1/2                  |
| Hidra Materiais Hidraulicos   | Joelho 1/2                        |
| Hidra Materiais Hidraulicos   | Junta 1/2                         |
+-------------------------------+-----------------------------------+
```

**Listagem 3**. Lista dos fornecedores e materiais oferecidos

Observe que aqui neste primeiro exemplo já estamos utilizando a definição do relacionamento entre a tabela de Fornecedores e Materiais que apresentamos no modelo de dados.

Este é um dos conceitos principais de bancos de dados e que, no momento em que escrevemos uma instrução SELECT, deve estar em nossa mente, ou ao nosso alcance para consulta.

Se cod_fornecedor é a coluna que define o relacionamento entre as tabelas, então esta restrição tem que ser [informada ao SGBD](https://www.devmedia.com.br/arquitetura-de-um-sgbd/25007) para que o relacionamento seja feito de forma correta. Foi por esse motivo que a cláusula WHERE foi inserida, informando que o cod_fornecedor das duas tabelas deve ser igual.

Observe também que colocamos “Nome Fornecedor” e “Nome Material” ao lado do nome do fornecedor e do material, separados apenas por um espaço.

Estes são considerados nomes alternativos para as colunas, ou alias. O primeiro e principal objetivo desses alias é dar um nome melhor a cada coluna.

Já descrevemos a lógica da existência da restrição “fornecedores.cod_fornecedor = materiais.cod_fornecedor”, mas não falamos ainda sobre o porquê esta restrição melhora a performance do comando.

Aqui já entra um conceito de implementação física do banco de dados e que normalmente é criado na base de dados de todas as aplicações antes dos desenvolvedores terem acesso a elas. Normalmente, o desenvolvedor não precisaria saber disso, pois o [administrador de banco de dados (DBA)](https://www.devmedia.com.br/artigo-sql-magazine-5-atividades-de-um-dba-e-questoes-sobre-performance/5068) já fez esse trabalho e já entrega a base de dados disponível com essa característica.

Este conceito é: para toda chave primária e toda chave estrangeira definidas em uma base de dados, devem existir índices de acesso correspondentes a elas. Sem nos aprofundarmos muito, conceitualmente os índices são aceleradores de acesso a uma tabela do banco de dados.

Quando o DBA sabe que uma tabela será muito acessada por uma coluna especificamente, ele cria um índice para esta coluna. Isso faz com que o SGBD encontre a linha que você precisa de maneira muito mais eficiente do que ler todas as linhas da tabela para trazer as que você solicitou.

E, por definição, o DBA cria índices para todas as chaves primárias e estrangeiras de todas as tabelas. Por isso, a performance do comando, quando é incluída a restrição pelas chaves primárias e secundárias, tem uma performance melhor. O SGBD saberá se resolver de uma forma mais eficiente.

Neste momento, também é importante notar que normalmente não se relacionam tabelas por colunas que não sejam as chaves primárias e secundárias das respectivas tabelas. Normalmente, este relacionamento não fará sentido se for feito por outras colunas. Mas, existirão exemplos onde, se você não conhecer o modelo de relacionamentos entre as tabelas de uma base de dados, você será tentado a estabelecer um relacionamento por outras colunas. Isso não só poderá trazer para você um resultado equivocado como também não terá uma boa performance.

Vamos introduzir neste ponto um novo conceito que ajudará muito na clareza e facilidade de leitura da instrução SELECT: o alias do nome das tabelas. O alias é um sinônimo que você atribui a uma tabela. É uma outra forma de se referenciar ao nome da mesma. Quando escrevemos a instrução, prefixamos as colunas com os nomes das respectivas tabelas.

Simples e claro até aqui porque nosso exemplo possui apenas duas colunas. Se a instrução possuísse vinte colunas e você tivesse que prefixar o nome das respectivas tabelas antes de cada uma delas, seria um desperdício de espaço e tempo e, além disso, a legibilidade do comando estaria prejudicada. Por isso existe o alias. Veja a seguir o mesmo exemplo reescrito utilizando um alias simples para cada tabela:

```
SELECT f.nome, m.nome
FROM fornecedores f, materiais m
WHERE f.cod_fornecedor = m.cod_fornecedor;
```

Note que colocamos uma letra “f” após apresentarmos a tabela fornecedores na cláusula FROM e da mesma forma, colocamos uma letra “m” após a tabela materiais. O “f” e “m” são os alias que criamos para as tabelas fornecedores e materiais respectivamente. Poderia ser qualquer nome ou abreviação, não apenas uma letra.

Dessa forma, apresentado após o nome de uma tabela ele será o alias para aquela tabela e o SGBD saberá de qual tabela você está falando quando fizer referência a ele. Assim, todas as referências às tabelas foram substituídas pelos alias correspondentes dentro da instrução, tornando o comando mais claro e legível.

### Inner Joins e Outer Joins

Continuando nossa definição de como “juntar” as informações de duas ou mais tabelas, existe um conceito muito importante que devemos apresentar antes de prosseguir.

No exemplo que fizemos até aqui, listamos os nomes dos fornecedores e respectivos nomes dos materiais. Note que listamos apenas as informações de fornecedores que estavam associadas aos respectivos materiais, isto é, quando os fornecedores e materiais existiam e eram iguais em ambas as tabelas. Este conceito de join é conhecido como inner join. Em um relacionamento desse tipo, somente serão listadas as linhas de fornecedores e materiais correspondentes que existam em ambas as tabelas.

Pode existir, porém, um caso onde gostaríamos de listar todas as linhas de uma tabela mesmo que não exista uma linha correspondente na tabela relacionada. Por exemplo, podemos querer a lista de todos os fornecedores junto com a soma de todos os pedidos realizados para eles, mas queremos todos os fornecedores mesmo aqueles que nunca tenhamos pedidos feitos para eles.

Para isso, precisamos fazer o join exatamente como realizado, porém com uma diferença sutil, para informar ao SGBD que queremos todos os fornecedores. Veja na **Listagem 4** como ficaria este exemplo utilizando o inner join.

```
SELECT f.nome, SUM(p.valor_total)
FROM fornecedores f, pedidos p
WHERE f.cod_fornecedor = p.cod_fornecedor
GROUP BY f.nome;
```

**Listagem 4**. Exemplo utilizando inner join

Se o comando for executado dessa forma, o SGBD retornará a lista dos fornecedores que já fizeram algum tipo de pedido e o seu total ao lado de cada um. Mas não é exatamente isso que queremos. Queremos todos os fornecedores, mesmo que nunca tenham feito pedidos. A alteração sutil está na **Listagem 5**.

```
SELECT f.nome, SUM(p.valor_total)
FROM fornecedores f, pedidos p
WHERE f.cod_fornecedor = p.cod_fornecedor(+)
GROUP BY f.nome;
```

**Listagem 5**. Exemplo utilizando outer join

Note o “(+)” ao lado do código do fornecedor, da tabela dos pedidos. Esta notação utilizada pelo SGBD Oracle, diz para que tenhamos todas as linhas de fornecedores, mesmo que não existam pedidos correspondentes, isto é, neste caso os pedidos sejam opcionais. O resultado da execução deste comando está na **Listagem 6**.

```
+---------------------------------------------+---------------------+
| nome                                         | SUM(p.valor_total) |
+---------------------------------------------+---------------------+
| ABC Materiais Eletricos                      |               NULL |
| Hidra Materiais Hidraulicos                  |              65.00 |
| HidraX Materiais ElÈtricos e Hidraulicos     |               NULL |
| XYZ Materiais de Escritorio                  |             150.00 |
+---------------------------------------------+---------------------+
```

**Listagem 6**. Exemplo de outer join

### Sintaxes diferentes para Inner e Outer Joins

Os exemplos apresentados funcionam e são a forma mais clara de se apresentar inner e outer joins. Utilizando sempre a cláusula WHERE para colocar as restrições de seleção, incluindo as restrições de join. Esta notação, conhecida como notação implícita, para inner joins existe para qualquer SGBD, porque ela não possui nenhuma diferença de uma cláusula WHERE tradicional. Porém, o exemplo de outer join apresentado utiliza a notação implícita oferecida pelo SQL do SGBD Oracle.

Outros fornecedores de SGBD possuem sintaxes diferentes para estas notações implícitas. O [SQL Server](https://www.devmedia.com.br/guia/sql-server/35720), por exemplo, utiliza o “*=” na comparação da cláusula WHERE para indicar que a tabela que está à esquerda é a que se quer todos os registros.

O SQL ANSI, porém, possui uma sintaxe adicional para joins, chamada de notação explícita, que padroniza a sintaxe de todos os tipos de join, onde os mesmos ficam representados na cláusula FROM, quando você declara as tabelas, e não na cláusula WHERE como apresentamos nos exemplos.

Geralmente, as pessoas não costumam gostar muito desta sintaxe porque ela fica cada vez mais complexa à medida que você acrescenta tabelas ao join e isso dificulta a leitura. Mas esta é uma questão de padronização, e devemos aqui apresentar esta sintaxe diferenciada, porque você certamente passará por ela ao longo da sua vida profissional. Existem ainda SGBDs que não implementam nenhum tipo de notação implícita para outer joins. Por este motivo, é mais importante ainda que você a conheça.

Nos exemplos que apresentamos, o inner join foi introduzido utilizando a notação implícita. Essa instrução seria representada na notação explícita da seguinte forma:

```
SELECT f.nome, m.nome
FROM fornecedores f INNER JOIN materiais m
     ON f.cod_fornecedor = m.cod_fornecedor;
```

Note que a cláusula WHERE foi suprimida nesse caso. Esta cláusula só apareceria se quiséssemos realmente fazer uma restrição dos dados por algum valor específico.

Nos exemplos que apresentamos, o outer join foi introduzido utilizando a notação implícita. Essa instrução seria representada na notação explícita conforme a **Listagem 7**.

```
SELECT f.nome, SUM(p.valor_total)
FROM fornecedores f LEFT OUTER JOIN pedidos p
     ON f.cod_fornecedor = p.cod_fornecedor
GROUP BY f.nome;
```

**Listagem 7**. Exemplo utilizando outer join explicitamente

Existem duas grandes vantagens na notação explícita: sua clareza e padronização. E, de novo, ela é comum em todos os SGBDs do mercado.

### Um exemplo de join de várias tabelas

O nosso exemplo de joins de várias tabelas será um relatório de todos os pedidos do mês de fevereiro incluindo as informações dos pedidos e os itens dos pedidos.

Este é um exemplo que parece bem simples, basta fazer um join das tabelas de pedidos e de itens dos pedidos e apresentar todas as informações de ambas as tabelas. Observe a **Listagem 8**.

```
SELECT p.*, ip.*
FROM pedidos p, itens_pedidos ip
WHERE p.num_pedido = ip.num_pedido AND
  year(data_pedido) = 2015 AND
  month(data_pedido) = 2;
```

**Listagem 8**. Exemplo de join simples

Temos o resultado dessa instrução na **Listagem 9**.

```
+--------------+------------------+--------------+---------------------+--------------+------+
 | num_pedido | cod_fornecedor | data_pedido | data_recebimento | quant_itens | valor_total |
+--------------+------------------+---------------+---------------------+--------------+------+
 |    111     | XYZ            | 2015-02-25  | 2015-03-31       |         200 |       75.00 |
 |    111     | XYZ            | 2015-02-25  | 2015-03-31       |         200 |       75.00 |
+--------------+------------------+---------------+---------------------+--------------+------+

 +--------------+---------------+----------------+----------------+
 | num_pedido | cod_material | quant_pedida | valor_unitario |
 +--------------+---------------+----------------+----------------+
 |    111     |         2345 |          100 |           0.50 |
 |    111     |         4569 |          100 |           0.25 |
 +--------------+---------------+----------------+----------------+
```

**Listagem 9**. Join simples de pedidos e itens dos pedidos (Sintaxe MySQL)

Mas não temos alguma coisa estranha? O que são esses vários códigos listados? Como saber quem são os fornecedores e o nome dos materiais de cada pedido? Estas informações não estão nas tabelas de pedidos e itens dos pedidos, elas estão nas tabelas de fornecedores e materiais.

Assim, para incluir essas informações temos que incluir as tabelas onde elas existem: o nome dos fornecedores na tabela de fornecedores, o nome dos materiais na tabela de materiais, etc.

O comando completo ficaria conforme a **Listagem 10**, e seu resultado conforme a **Listagem 11**.

```
SELECT f.nome, p.num_pedido, p.data_pedido, p.data_recebimento,
  p.quant_itens, p.valor_total, m.nome, ip.quant_pedida,
  u.nome, ip.valor_unitario
FROM pedidos p,
  itens_pedidos ip,
  fornecedores f,
  materiais m,
  unidades u
WHERE p.num_pedido = ip.num_pedido AND
  p.cod_fornecedor = f.cod_fornecedor AND
  ip.cod_material = m.cod_material AND
  m.cod_unidade = u.cod_unidade AND
    year(data_pedido) = 2015 AND
    month(data_pedido) = 2;
```

**Listagem 10**. Exemplo de join de pedidos e itens

```
+------------------------------+--------------+---------------+--------------------+-------+
| nome                        | num_pedido | data_pedido | data_recebimento | quant_itens |
+------------------------------+--------------+---------------+--------------------+-------+
| XYZ Materiais de Escritorio |        111 | 2015-02-25  |   2015-03-31     |         200 |
| XYZ Materiais de Escritorio |        111 | 2015-02-25  |   2015-03-31     |         200 |
+------------------------------+--------------+---------------+--------------------+-------+

+------------+--------------+---------------+-----------+----------------+
| valor_total | nome         | quant_pedida | nome      | valor_unitario |
+------------+--------------+---------------+-----------+----------------+
|       75.00 | Caneta Azul |          100  | Unidades  |           0.50 |
|       75.00 | Lapis HB    |          100  | Unidades  |           0.25 |
+-------------+-------------+---------------+------------+----------------+
```

**Listagem 11**. Join de pedidos e itens dos pedidos complementado com nomes dos fornecedores e materiais

Note que para apresentar todas as informações textuais que precisávamos, tivemos que unir cinco tabelas, três além das tabelas bases iniciais. Note também que existe uma condição de join para cada uma delas.

Seguindo o modelo de relacionamentos das tabelas, temos a tabela Pedidos que se relaciona com a tabela itens de pedidos. Isso está representado na cláusula WHERE por “p.num_pedido = ip.num_pedido”.

Para obter o nome dos fornecedores, tivemos que incluir o relacionamento de pedidos e fornecedores representado por “p.cod_fornecedor = f.cod_fornecedor”.

Já os materiais aparecem ao nível dos itens dos pedidos e para relacioná-los incluímos “ip.cod_material = m.cod_material”. E, finalmente, os itens dos pedidos também possuem unidades, que são descritas na tabela de unidades, que se relacionam com os materiais e foram representados por “m.cod_unidade = u.cod_unidade”.

Observe também que incluímos na lista dos campos a retornar o nome dos fornecedores (f.nome), materiais (m.nome), e unidades (u.nomes).

O resultado apresentado não tem a aparência de um relatório, não possui uma formatação apresentável e também todas as informações da tabela de pedidos estão aparecendo repetidamente em cada linha correspondente a cada item do pedido. Isso é assim mesmo.

Esses são os dados que você utilizará em seu programa, ou no seu software gerador de relatórios, para apresentação. Os dados repetidos serão agrupados pelo seu programa, ou pelo gerador de relatórios, para serem apresentados uma única vez. A sintaxe que utilizamos para as funções de ano e mês, e que são a nossa única restrição real dos dados que queremos, [são do SGBD MySQL](https://www.devmedia.com.br/guia/mysql/34335).

Como ficaria este join utilizando a notação explícita? Observe a **Listagem 12**.

```
SELECT f.nome, p.num_pedido, p.data_pedido, p.data_recebimento,
   p.quant_itens, p.valor_total, m.nome, ip.quant_pedida,
   u.nome, ip.valor_unitario
FROM pedidos p
INNER JOIN fornecedores f    ON p.cod_fornecedor = f.cod_fornecedor
INNER JOIN itens_pedidos ip ON p.num_pedido = ip.num_pedido
INNER JOIN materiais m       ON ip.cod_material = m.cod_material
INNER JOIN unidades u        ON m.cod_unidade = u.cod_unidade
WHERE year(data_pedido) = 2015 AND
month(data_pedido) = 2;
```

**Listagem 12**. Exemplo utilizando join explicitamente

Utilizar a notação implícita ou explícita pode ser uma questão de gosto, de clareza do texto da instrução ou uma questão de padronização. Algumas empresas podem solicitar que os desenvolvedores sigam um padrão na elaboração das queries. Todos, porém, funcionam da mesma forma.

### Por que o Join?

Aqui entra um questionamento interessante que povoa as mentes de muitos desenvolvedores: Por que então não fazer duas queries separadas em vez de apenas uma? Por que agrupar todas as informações de pedidos e itens dos pedidos em uma única query, mais complexa, que também me trará um esforço de programação razoável, se eu posso ter duas queries, uma delas, um SELECT, listando apenas os pedidos, e para cada pedido uma segunda query para selecionar os seus itens?

A resposta para este questionamento é uma só: Performance! A cada vez que o SGBD tem uma nova query para execução, ele tem que seguir os seguintes passos, e que podem variar de um fabricante para outro:

1. Avaliar a query sintaticamente e semanticamente;
2. Montar o plano de acesso aos dados, isto é, decidir como os dados serão acessados e recuperados para você;
3. Executar a query.

Os dois primeiros itens são também conhecidos como “parse” da instrução. Eles normalmente consomem décimos ou centésimos de segundo. O terceiro item, a execução da query, é o que normalmente consome um pouco mais, mas normalmente não mais que alguns poucos segundos se a instrução estiver bem escrita.

A princípio, somente com estas informações, não haveria porque fazer o join. Quem não pode aguardar dois ou três segundos por um relatório? O argumento de performance teria ido por água abaixo se não houvessem a seguintes perguntas: Quantos pedidos vamos tratar? Quantos itens existem em média para cada pedido?

Se a resposta for uma dezena de pedidos, realmente não teremos problemas de performance. Para falar a verdade, não precisaríamos nem desenvolver uma aplicação para isso. Mas, e se a resposta for que temos pedidos na ordem dos milhares por mês? Tomando como exemplo um montante de aproximadamente dois mil pedidos mensais, com uma média de cinco itens por pedido, se separássemos este join em duas queries, uma de pedidos e outra para os itens de cada pedido, teríamos a primeira sempre executando uma única vez.

Sem problemas. Mas, a segunda seria executada duas mil vezes. Se esta segunda instrução demorar um segundo para executar, o tempo total de execução da segunda query 2000 vezes seria de 2000 segundos! Normalmente ninguém está disposto a aguardar mais de 30 minutos para ter o resultado de um relatório simples como este.

Mas ainda vamos ter alguém que afirme: mas o join de todas essas tabelas numa única query vai demorar mais para executar que se tivéssemos duas queries separadas.

Ainda assim, por que juntar? E a resposta é: sim, o join vai demorar um pouco mais. Mas se ele demorar mais do que 3 segundos, já poderíamos dizer que tem algo mais errado com o seu banco de dados.

Portanto, o SGDB executar para você o join de várias tabelas e te retornar o resultado todo de uma vez, em 99% dos casos será mais rápido que executar várias queries repetidamente. Portanto, o esforço vale a pena.

### Outras sintaxes de Outer Joins

No exemplo de outer join que mostramos, a restrição f.cod_fornecedor = p.cod_fornecedor (+), da sintaxe implícita, fazia com que trouxéssemos todos os fornecedores, mesmo que não houvessem pedidos correspondentes. Esta característica foi representada na sintaxe explícita, na cláusula FROM, dessa forma: FROM fornecedores f LEFT OUTER JOIN pedidos p ON f.cod_fornecedor = p.cod_fornecedor.

Mas e se quiséssemos o contrário? Se quiséssemos ter a lista de todos os pedidos, mesmo que não existissem fornecedores correspondentes? Isso não faz muito sentido no nosso exemplo, mas tecnicamente é possível. Você já poderia dizer que, na forma implícita, basta colocar o (+) do outro lado da igualdade, fazendo f.cod_fornecedor(+) = p.cod_fornecedor. O que é verdade e funciona. Aí teríamos a instrução completa desta forma:

```
SELECT f.nome, SUM(p.valor_total)
FROM fornecedores f, pedidos p
WHERE f.cod_fornecedor(+) = p.cod_fornecedor;
```

Mas temos ainda a forma explícita de notação do outer join. E nessa forma ainda teríamos duas formas de representá-lo. A primeira, invertendo as tabelas Fornecedores e Pedidos na cláusula FROM, em relação ao exemplo apresentado:

```
SELECT f.nome, SUM(p.valor_total)
FROM pedidos p LEFT OUTER JOIN fornecedores f
   ON f.cod_fornecedor = p.cod_fornecedor;
```

E a segunda utilizando uma outra sintaxe do outer join, o right outer join. Esta sintaxe diz ao SGBD para fazer exatamente o que precisamos, considerar a tabela da direita, pedidos, como a que terá todas as suas linhas apresentadas, independentemente de existirem fornecedores correspondentes. A instrução seria então escrita desta forma:

```
SELECT f.nome, SUM(p.valor_total)
FROM fornecedores f RIGHT OUTER JOIN pedidos p
   ON f.cod_fornecedor = p.cod_fornecedor;
```

Em todos os exemplos apresentados teríamos o mesmo resultado, e a performance da instrução seria exatamente a mesma. São apenas formas diferentes de se escrever uma mesma instrução.

Mas as sintaxes para diferentes possibilidades de joins não terminam aqui. Temos ainda o full outer join. É importante salientar que este não é suportado por alguns fabricantes de SGBDs.

O sentido do full outer join para nós seria:

1. Queremos todos os fornecedores com seus pedidos correspondentes, se existirem, mas sempre todos os fornecedores. Esse foi o nosso exemplo inicial do left outer join;
2. Queremos também todos os pedidos com seus fornecedores correspondentes, se existirem, mas sempre todos os pedidos. Esse foi o nosso exemplo do right outer join.

No full outer join nós queremos as duas condições simultaneamente. E a nossa instrução seria escrita conforme a **Listagem 13**.

```
SELECT f.nome, SUM(p.valor_total)
FROM fornecedores f FULL OUTER JOIN pedidos p
     ON f.cod_fornecedor = p.cod_fornecedor
GROUP BY f.nome;
```

**Listagem 13**. Exemplo utilizando full outer join

O resultado dessa instrução está representado na **Listagem 14**.

```
+-------------------------------------------+-------------------------+
| nome                                        | SUM(p.valor_total)   |
+--------------------------------------------+------------------------+
| ABC Materiais Eletricos                     |               NULL   |
| Hidra Materiais Hidraulicos                 |              65.00   |
| HidraX Materiais ElÈtricos e Hidraulicos    |               NULL   |
| XYZ Materiais de Escritorio                 |             150.00   |
+---------------------------------------------+-----------------------+
```

**Listagem 14**. Resultado de um full outer join

Mas e se o full outer join não for implementado pelo SGBD que você usa? A solução é fazermos as duas queries independentes, uma para cada um dos joins definidos de forma separada anteriormente e depois utilizar a nossa teoria de conjuntos para juntar o resultado de ambas.

Para isso, vamos apresentar como fazer a união dos dois conjuntos de dados do exemplo anterior do full outer join. Vamos introduzir o conceito da união de queries.

### O operador UNION

Nosso conceito já está apresentado no exemplo do full outer join. Precisamos unir o resultado de duas queries, ou dois conjuntos de dados. A união desses conjuntos de dados é realizada pelo operador union. Ele une o resultado de duas instruções SELECT.

Mas, para que isso aconteça, temos que ter um único princípio básico: o resultado de ambas as queries têm que ter as mesmas colunas, na mesma sequência, e do mesmo tipo.

O SGBD não tem como saber se semanticamente os dados são os mesmos, por isso ele avalia apenas se o número de colunas é igual e se o tipo das colunas correspondentes em ambas as queries é igual ou equivalente.

Dessa forma, poderíamos simular o full outer join utilizando o operador union conforme a **Listagem 15**.

```
SELECT f.nome, SUM(p.valor_total)
FROM fornecedores f LEFT OUTER JOIN pedidos p
  ON f.cod_fornecedor = p.cod_fornecedor
GROUP BY f.nome
UNION
SELECT f.nome, SUM(p.valor_total)
FROM fornecedores f RIGHT OUTER JOIN pedidos p
  ON f.cod_fornecedor = p.cod_fornecedor
GROUP BY f.nome;
```

**Listagem 15**. Exemplo utilizando union

O resultado da execução deste comando será exatamente o mesmo resultado do full outer join apresentado na **Listagem 14**.

O operador union possui ainda a opção all. O operador union, quando escrito union all, simplesmente junta os dois resultados das duas queries para fornecer para você um resultado único, de um único SELECT, porém sem nenhuma análise sobre os dados unidos.

Existem casos onde o resultado da primeira e da segunda queries podem apresentar linhas com os mesmos valores em todas as colunas, isto é, linhas duplicadas. O operador union puro, sem a opção all, elimina essas duplicidades para você.

É como na teoria dos conjuntos. Se tivéssemos dois conjuntos de frutas:

- Frutas cítricas, com: laranja, limão e maracujá;
- Frutas verdes, com: pera, uva verde e limão;

O resultado do union all seria: laranja, limão, maracujá, pera, uva verde e limão. Note que o limão apareceu duas vezes. Pode ser que você queira esse tipo de resultado, para quantificar, por exemplo, quantas vezes uma fruta aparece nos seus conjuntos.

Mas pode ser que não, que você queira o conjunto final sem essas duplicidades. Aí, nesse caso, o resultado do union puro seria o ideal para você, porque traria como resposta a lista de frutas sem repetição: laranja, limão, maracujá, pera e uva verde.

A diferença dos resultados você já entendeu. O que é importante dizer agora é que, para retirar as duplicidades dos dois conjuntos, o operador union puro precisará trabalhar mais, e gastará mais tempo e processamento para realizar seu trabalho. Portanto, antes de utilizá-lo, tenha certeza de que é esse o resultado que você quer.

O exemplo apresentado ilustrou como o operador de queries union funciona unindo o resultado de duas queries. Mas e se quiséssemos que o resultado final ainda fosse classificado alfabeticamente pelo nome do fornecedor?

Se colocássemos a [cláusula ORDER BY](https://www.devmedia.com.br/sql-order-by/41225) dentro de cada uma das queries, teríamos cada uma classificada, mas não teríamos o resultado final classificado. Na realidade, os SGBDs nem permitem que você coloque cláusulas ORDER BY em queries que serão ainda unidas.

A solução nesse caso é colocar o ORDER BY no fim da última query. Assim, nosso resultado ficaria conforme a **Listagem 16**.

```
SELECT f.nome, SUM(p.valor_total)
FROM fornecedores f LEFT OUTER JOIN pedidos p
  ON f.cod_fornecedor = p.cod_fornecedor
GROUP BY f.nome
UNION
SELECT f.nome, SUM(p.valor_total)
FROM fornecedores f RIGHT OUTER JOIN pedidos p
  ON f.cod_fornecedor = p.cod_fornecedor
GROUP BY f.nome
ORDER BY f.nome;
```

**Listagem 16**. Exemplo utilizando ORDER BY

Simples, rápido e funcional. Teremos o resultado final da união classificado pelo nome do fornecedor.

### O operador INTERSECT

E vamos caminhando na teoria dos conjuntos. Vamos apresentar agora o operador de interseção. A interseção de dois conjuntos é o conjunto do que existir de comum, coincidente ou igual, entre os dois primeiros.

Utilizando o exemplo de conjuntos apresentado, para os conjuntos de frutas cítricas e frutas verdes, o resultado da sua interseção seria apenas a fruta limão, que aparece em ambos os conjuntos iniciais.

Vamos então concretizar nosso conhecimento em interseção de queries apresentando duas queries de exemplo:

1. A lista dos fornecedores dos quais já compramos mais de R$50 em pedidos, e;
2. A lista dos fornecedores de quem não compramos há mais de um ano;

O resultado que vamos querer da sua interseção serão os resultados de ambas, portanto serão os fornecedores de quem compramos mais de R$50 em pedidos e, ao mesmo tempo, não compramos dele há mais de um ano.

Vamos por partes. Para termos o resultado da primeira consulta precisaríamos executar o comando da **Listagem 17**.

```
SELECT f.nome
FROM fornecedores f, pedidos p
WHERE f.cod_fornecedor = p.cod_fornecedor
GROUP BY f.nome
HAVING SUM(p.valor_total)>50;
```

**Listagem 17**. Exemplo de consulta de lista dos fornecedores com o total de pedidos superior a R$50

Podemos ver seu resultado na **Listagem 18**. Note que fizemos um join de fornecedores e pedidos (f.cod_fornecedor = p.cod_fornecedor) e agrupamos pelo nome do fornecedor (GROUP BY f.nome).

Note que não apresentamos a soma no resultado da query. Só queremos a lista dos fornecedores com esta condição. Temos também que filtrar os fornecedores cujo total de pedidos seja superior ao valor estabelecido.

Se parássemos antes de filtrar, não teríamos só os dos que compramos mais de R$50, teríamos todos. Portanto, precisamos acrescentar a cláusula HAVING para filtrar o resultado da agregação.

```
+----------------------------------+
| nome                             |
+----------------------------------+
| Hidra Materiais Hidraulicos      |
| XYZ Materiais de Escritorio      |
+----------------------------------+
```

**Listagem 18**. Lista dos fornecedores com o total de pedidos superior a R$50

Para a segunda consulta precisaríamos da consulta apresentada na **Listagem 19**.

```
SELECT f.nome
FROM fornecedores f, pedidos p
WHERE f.cod_fornecedor = p.cod_fornecedor AND
  p.data_pedido < current_date - interval 1 year;
```

**Listagem 19.** Exemplo de consulta de lista dos fornecedores sem pedidos a mais de um ano

O resultado desta query está na **Listagem 20**. Note que fizemos um join simples entre Fornecedores e Pedidos, semelhante a query anterior e que filtramos os pedidos com a data de pedido anterior a um ano em relação à data atual (p.data_pedido < current_date - interval 1 year). Esta sintaxe é do SGBD MySQL.

```
+----------------------------------+
| nome                             |
+----------------------------------+
| Hidra Materiais Hidraulicos      |
+----------------------------------+
```

**Listagem 20**. Lista dos fornecedores sem pedidos há mais de um ano

Agora o que precisamos fazer é a interseção das duas instruções, conforme a **Listagem 21**.

```
SELECT f.nome
FROM fornecedores f, pedidos p
WHERE f.cod_fornecedor = p.cod_fornecedor
GROUP BY f.nome
HAVING SUM(p.valor_total)>50
INTERSECT
SELECT f.nome
FROM fornecedores f, pedidos p
WHERE f.cod_fornecedor = p.cod_fornecedor AND
  p.data_pedido < current_date - interval 1 year;
```

**Listagem 21**. Exemplo de intersecção de duas queries

O resultado está apresentado na **Listagem 22**, e como podemos observar, apenas o fornecedor que está presente no resultado das duas consultas é o resultado da interseção.

Porque não fizemos uma única query para obter o resultado esperado das duas condições? É possível se obter o mesmo resultado de outras formas. Utilizamos o exemplo apenas para demonstrar o [uso do operador INTERSECT](https://www.devmedia.com.br/operacoes-com-conjuntos-no-sql-server/25357#2). Tudo que vai nos guiar em utilizar uma ou outra forma de se montar uma instrução SELECT é: clareza no detalhamento da instrução, para que ela seja legível e compreensível por quem mais precisar lê-la, e performance. Não adianta escrevermos uma instrução legível, mas que demora longos minutos para executar.

```
+---------------------------------+
| nome                            |
+---------------------------------+
| Hidra Materiais Hidraulicos     |
+---------------------------------+
```

**Listagem 22**. Resultado do INTERSECT de duas queries

### O operador MINUS

Já que estamos associando a teoria de conjuntos à construção de consultas de acesso às bases de dados, vamos mostrar o último operador de queries do nosso artigo. Ele opera a subtração de queries, ou subtração de conjuntos.

Tomaremos como base exatamente as mesmas duas queries do exemplo da interseção de consultas. Mas dessa vez queremos como resultado apenas os fornecedores de quem já compramos mais de R$50 em Pedidos, e que estão ativos, isto é, de quem fizemos compras mais recentes do que um ano.

Ou, para utilizar a mesma query e atender ao nosso exemplo de subtração de consultas, que este não esteja na lista dos fornecedores de quem não compramos a mais de um ano.

Como as queries dos exemplos um e dois já foram apresentadas e detalhadas, o que precisamos agora é somente a interação entre elas de forma que o resultado da segunda seja excluído, subtraído, do resultado da primeira. Observe a **Listagem 23**.

```
SELECT f.nome
FROM fornecedores f, pedidos p
WHERE f.cod_fornecedor = p.cod_fornecedor
GROUP BY f.nome
HAVING SUM(p.valor_total)>50
MINUS
SELECT f.nome
FROM fornecedores f, pedidos p
WHERE f.cod_fornecedor = p.cod_fornecedor AND
      p.data_pedido < current_date - interval 1 year;
```

**Listagem 23**. Exemplo utilizando minus

Apresentamos o resultado na **Listagem 24**, onde podemos ver que estão exatamente os fornecedores resultantes da primeira consulta, excluídos os existentes na segunda consulta.

```
+-----------------------------+
| nome                        |
+-----------------------------+
| XYZ Materiais de Escritorio |
+-----------------------------+
```

**Listagem 24**. Resultado do MINUS de duas queries

É importante salientar que alguns SGBDs também não implementam o operador MINUS, mas, novamente, como existem inúmeras maneiras de se escrever uma query, e estamos utilizando o SGBD MySQL nestes exemplos, vou apresentar uma forma de contornar esta situação.

Vamos apresentar um outro operador e uma outra forma de se escrever duas queries, uma dentro da outra. Vamos apresentar rapidamente o conceito de subqueries com o exemplo da **Listagem 25** para suprir a falta do MINUS no MySQL.

```
SELECT f.nome
FROM fornecedores f, pedidos p
WHERE f.cod_fornecedor = p.cod_fornecedor AND
  f.nome NOT IN
    (SELECT f.nome
     FROM fornecedores f, pedidos p
     WHERE f.cod_fornecedor = p.cod_fornecedor AND
       p.data_pedido < current_date - interval 1 year
    )
GROUP BY f.nome
HAVING SUM(p.valor_total)>50;
```

**Listagem 25**. Exemplo suprindo a falta do operador minus

Note que uma query está explicitamente dentro da outra. A interpretação aqui seria: queremos a lista dos fornecedores dos quais já compramos mais de R$50 em Pedidos, porém, este resultado não pode conter os nomes dos fornecedores de quem não compramos a mais de um ano.

O operador NOT IN faz exatamente isso, exclui os fornecedores que estão no conjunto retornado pela query seguinte. Observe também que a query dos fornecedores de quem não compramos a mais de um ano está entre parêntesis.

O objetivo dos parêntesis é separar esta subquery da query mais externa, para que o SGBD não misture as cláusulas da primeira query com as cláusulas da segunda. E desta forma, resolvemos a ausência do operador MINUS. O resultado de ambos os exemplos é exatamente o mesmo.

### Auto Join

É importante apresentar também um conceito de join que, embora não seja muito popular, ocorre de maneira mais comum do que possamos imaginar. O “auto-join”, ou o join de uma tabela com ela mesma.

Quando vemos este exemplo de join pela primeira vez, achamos que isso seria apenas uma teoria, não faria sentido na vida real. Que seria uma das coisas que provavelmente nós nunca utilizaríamos profissionalmente e que estavam nos ensinando só para que nós soubéssemos que existia. Vamos apresentar um exemplo, diferente do modelo de dados apresentado, e que se utiliza do autorelacionamento para sua implementação.

Imagine que tivéssemos uma tabela de funcionários de uma empresa. Nesta tabela tem que estar cadastrados todos os funcionários dessa empresa, incluindo a coordenação, gerência, diretoria, etc.

E queremos representar nesse modelo de dados quem é o chefe de quem. Em outras palavras, queremos conseguir representar quem é o coordenador de um ou um grupo de funcionários, quem é o gerente desse coordenador, quem é o diretor desse gerente, etc. E estamos assumindo que cada funcionário possui apenas um chefe.

Esta implementação pode ser feita de várias maneiras, mas a maneira que escolhemos foi uma das mais simples: dizer com linha de cadastro do funcionário qual é o chefe dele.

Para representar isso no modelo de dados, precisaríamos apenas incluir uma coluna a mais nessa tabela, representando o código do funcionário que é o chefe desse funcionário.

Apresentaremos as colunas dessa tabela para o entendimento ficar mais fácil conforme a **Figura 2**.

![Representação da tabela Funcionários](https://arquivo.devmedia.com.br/REVISTAS/sql/imagens/132/4/2.png)**Figura 2**. Representação da tabela Funcionários

Representamos apenas algumas colunas como exemplo da tabela de funcionários. Observe que existe uma coluna chamada matricula_chefe, que é onde será indicado qual é o chefe daquele funcionário. O funcionário chefe terá uma linha nesta mesma tabela para defini-lo.

Neste caso, como temos uma coluna da tabela que faz referência a uma outra linha da mesma tabela, dizemos que este é um auto relacionamento. Isto é, a tabela se relaciona com ela mesma e, no Modelo de Entidades e Relacionamentos, este auto relacionamento aparece como uma seta apontando para a própria tabela.

Vamos colocar alguns dados nessa tabela para que nosso exemplo fique mais concreto. Na **Listagem 26** você poderá encontrar estes dados.

```
+-----------+-----------+---------+-----------------+
| matricula | nome      | cargo    | matricula_chefe|
+-----------+-----------+---------+-----------------+
|         1 | Marcus    | Gerente  | 2              |
|         2 | Marcelo   | Diretor  | NULL           |
|         3 | Fernando  | DBA      | 1              |
|         4 | Leticia   | DBA      | 1              |
+-----------+----------+---------+-----------------+
```

**Listagem 26**. Dados de exemplo para um auto join

Agora, voltando ao auto join, como faríamos para listar o nome dos funcionários, seus respectivos cargos e o nome do chefe desse funcionário com o cargo do chefe? A solução para nossa pergunta está na instrução da **Listagem 27** e seu resultado na **Listagem 28**.

```
SELECT f.nome, f.cargo,
  chefe.nome, chefe.cargo
FROM funcionarios f, funcionarios chefe
WHERE f.matricula_chefe = chefe.matricula;
```

**Listagem 27**. Exemplo auto join

```
+----------+---------+---------+----------+
| nome     | cargo   | nome    | cargo    |
+----------+---------+---------+----------+
| Fernando | DBA     | Marcus  | Gerente  |
| Leticia  | DBA     | Marcus  | Gerente  |
| Marcus   | Gerente | Marcelo | Diretor  |
+----------+---------+---------+---------+
```

**Listagem 28**. Resultado do auto join

Note que a tabela Funcionários aparece duas vezes no mesmo comando SELECT. Uma vez fazendo o papel da tabela de funcionários que queremos listar e a segunda vez, fazendo o papel da tabela dos chefes dos funcionários.

Colocamos um alias bem sugestivo na segunda representação da tabela funcionários para ficar claro que, para a segunda estamos falando dos chefes, e não dos funcionários (“funcionarios chefe”).

Perceba também que, da mesma forma que fizemos em todos os exemplos desse artigo, temos que representar como estas duas tabelas se relacionam.

Para isso, tivemos que dizer que a tabela de funcionários se relaciona com a tabela de chefes dos funcionários através do cod_funcionario_chefe dessa forma “f.cod_funcionario_chefe = chefe.cod_funcionario”. É como se tivéssemos realmente duas tabelas.

No resultado do exemplo do join, um inner join, observe que temos apenas três funcionários listados, e não quatro como é a lista original dos funcionários. Observe que o funcionário que tem cargo de diretor não aparece no resultado com o nome do seu chefe ao lado.

Note que nos dados originais o diretor apresenta um NULL no lugar da matrícula do chefe dele, isto é, ele não tem chefe, ou o chefe dele não está definido.

Para o exemplo, onde queríamos todos os funcionários com seus chefes ao lado, o resultado pode estar correto. Mas e se quiséssemos todos os funcionários com ou sem chefes? A solução seria um outer join, onde incluiríamos todos os funcionários e os seus chefes seriam opcionais.

### Cross Join

Este é o último conceito de join que vamos apresentar nesse artigo. Vamos mostrar este conceito aqui apenas para que você saiba que ele existe. Nunca vimos uma utilidade prática para ele e encontrá-lo numa query é motivo para pensarmos que estamos à frente de um erro. Você vai entender o porquê.

Na sua notação implícita, o cross join das tabelas de fornecedores e pedidos seria assim:

```
SELECT fornecedores.nome, materiais.nome
FROM fornecedores, materiais;
```

Você sentiu falta de alguma coisa? Este foi o nosso primeiro exemplo, onde dissemos que faltava a condição de join e que neste caso, teríamos como retorno cada linha da tabela fornecedores listados ao lado de todas as linhas da tabela de materiais, isto é, como se multiplicássemos as linhas das duas tabelas. E concluímos que este join não faria sentido dessa forma. Mas esta é a definição do cross join!

Também podemos chamar este tipo de join de produto cartesiano entre as tabelas participantes. De novo a teoria de conjuntos.

Em alguns SGBDs ele é considerado como um erro de construção da instrução SELECT. Normalmente, sua execução não é impedida, mas em uma avaliação do plano de execução dessa query, isto é, o plano de como os dados serão recuperados para serem apresentados para você, certamente teríamos uma sinalização de que o produto cartesiano existe para chamar sua atenção para este possível erro na construção do join.

Se existe uma notação implícita, existe também uma notação explícita. Que seria assim:

```
SELECT fornecedores.nome, materiais.nome
FROM fornecedores CROSS JOIN materiais;
```

Onde fica explicitamente claro que queremos o produto cartesiano das duas tabelas. Mais uma vez, nem todos os SGBDs implementam este tipo de join.

Certamente, joins são muito utilizados no dia a dia profissional de desenvolvedores de sistemas, e você certamente já construiu ou construirá vários. Tenha em mente três conceitos que destacamos ao longo desse artigo:

1. Atingir o objetivo proposto;
2. Legibilidade;
3. Performance

Um join deve ser escrito de forma a atender o resultado que você espera, que você consiga identificá-lo, lê-lo, de maneira direta e objetiva e ele deve executar de maneira eficiente. Esperamos ter dado base e argumentos suficientes para que você construa joins corretos e eficientes.

#### Confira também

[![Cursos de SQL e Banco de Dados](https://arquivo.devmedia.com.br/cursos/imagem/curso_consultas-basicas-em-sql_1902.jpg)Cursos de SQL e Banco de DadosCurso](https://www.devmedia.com.br/cursos/banco-de-dados)

[![Curso de SQL](https://arquivo.devmedia.com.br/cursos/imagem/curso_1895.jpg)Curso de SQLCurso](https://www.devmedia.com.br/curso/curso-de-sql/1895)

[![Aprenda SQL](https://arquivo.devmedia.com.br/cursos/imagem/curso_409.jpg)Aprenda SQLGuia](https://www.devmedia.com.br/sql/)