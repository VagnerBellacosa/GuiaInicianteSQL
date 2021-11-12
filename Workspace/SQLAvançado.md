# SQL Avançado

## Veja nesse artigo algumas funções avançadas de SQL, assim como trabalhar com datas, conversões e relacionamento entre tabelas.

Após vermos algumas [funções básicas em SQL](http://www.devmedia.com.br/sql-basico/28877), podemos começar a nos aprofundar mais no assunto.

Como sabemos o SQL é uma linguagem de comunicação com Banco de dados e é a mais utilizada do mundo.

### Trabalhando com Funções

#### NVL

A função NVL é utilizada para substituir O código abaixo apenas para exibição. o valor nulo do campo pesquisado por outro valor determinado.

O código abaixo seleciona o campo punit dos registros da tabela pcmov, atribuindo valor 0 se for nulo.

**Listagem 1:** NVL

```
select nvl(punit,0) from pcmov
```

#### NVL2

A função NVL2 é utilizada para substituir O código abaixo apenas para exibição. o valor do campo, sendo este nulo ou não.

O código abaixo seleciona o campo vpago dos registros da tabela pcprest, atribuindo valor 0 se NÃO for nulo, e atribuindo 2 se for nulo.

**Listagem 2**: NVL2

```
select nvl2(vpago,0,2) from pcprest
```

#### NULLIF

A função NULLIF compara dois valores. Se os valores forem iguais, a função retornará valor nulo. Se os valores dos campos valor e vpago forem iguais, a função retornará valor nulo. Se os valores desses campos forem diferentes, a função retornará o primeiro valor.

**Listagem 3**: NULLIF

```
select valor, vpago, nullif(valor, vpago) from pcprest;
```

#### COALESCE

A função COALESCE verifica os valores da expressão e retorna o primeiro valor não nulo.

O código abaixo seleciona dos registros da tabela pcprest, dentre os campos vldesconto, vpago e valor o primeiro que apresentar valor não nulo.

**Listagem 4**: Usando a função COALESCE

```
select coalesce(vldesconto, vpago, valor) from pcprest;
```

#### DECODE

A função Decode tem como objetivo ‘decodificar’ o valor do campo selecionado, atribuindo um valor de resposta para cada valor de condição estipulado.

O código abaixo mostra como selecionar dos registros da tabela pcmov o campo numnota, e exibe mensagem de acordo com as condições: se o campo numnota for igual a 100, exibe ‘CEM’, se for 200, exibe ‘DUZENTOS’, se for 300, exibe ‘TREZENTOS’, se não for nenhum dos anteriores, exibe ‘NENHUM’.

**Listagem 5**: Usando função decode

```
select numnota, decode(numnota, 100, ‘CEM’, 200, ‘DUZENTOS’, 300, ‘TREZENTOS’, ‘NENHUM’) resultado from pcmov;
```

#### CASE

A função CASE é utilizada para determinar a exibição do resultado a partir de condições especificadas dentro da função.

O código abaixo seleciona dos registros da tabela pcmov o campo numnota, e exibe mensagem de acordo com as condições: caso o campo numnota seja menor que 100, exibe a mensagem ‘MENOR QUE 100’, caso o campo numnota esteja entre 101 e 200, exibe a mensagem ‘ENTRE 101 E 200’, caso contrário, exibe a mensagem ‘MAIOR QUE 200’.

**Listagem 6:** Usando Case

```
select numnota, case        when numnota < 100 then ‘MENOR QUE 100’
                                         when numnota between 101 and 200 then ‘ENTRE 101 E 200’
                                         else ‘MAIOR QUE 200’ end “RESULTADO”
from pcmov;
```

***Obs**.: Para melhor entendimento da função: para todo ‘case’ deve existir um ‘end’, e para todo ‘when’ deve existir um ‘then’. O ‘else’ existe sozinho, como última alternativa da função.*

#### LOWER

O código abaixo seleciona dos registros da tabela pcempr os campos matricula e nome, convertendo o conteúdo do campo nome para que seja exibido todo em caracterees minúsculos.

**Listagem 7**: Usando função Lower

```
select matricula, lower(nome) from pcempr;
```

#### UPPER

O código abaixo seleciona dos registros da tabela pcempr os campos matricula e nome, convertendo o conteúdo do campo nome para que seja exibido todo em caracterees maiúsculos.

**Listagem 8**: Usando função Upper

```
select matricula, upper(nome) from pcempr;
```

**INITCAP**

O código abaixo seleciona dos registros da tabela pcempr os campos matricula e nome, convertendo o conteúdo do campo nome para que seja exibido com as iniciais em caracterees maiúsculos.

**Listagem 9:** Usando função initcap

```
select matricula, initcap(nome) from pcempr;
```

#### SUBSTR

A função SUBSTR tem como objetivo extrair parte do conteúdo do campo selecionado, de acordo com os parâmetros informados na função, independente do tipo do campo (numérico, texto ou data).

O código abaixo seleciona dos registros da tabela pcempr os campos matricula e nome, exibindo, do campo nome, apenas o conteúdo a partir do primeiro caractere, os próximos 5 caracterees.

**Listagem 10**: Usando Substr

```
select matricula, substr(nome,1,5) from pcempr;
```

#### LENGTH

A função LENGTH tem como objetivo exibir o tamanho do valor que está gravado no campo selecionado.

O código abaixo seleciona dos registros da tabela pcempr os campos matricula e nome, e exibe, logo em seguida, o tamanho do conteúdo do campo nome.

**Listagem 11**: Usando Length

```
select matricula, nome, length(nome) from pcempr;
```

#### REPLACE

A função REPLACE tem como objetivo possibilitar a substituição de caracteres específicos por outros.

O código abaixo seleciona dos registros da tabela pcempr os campos matricula e nome, e exibe, logo em seguida, o campo nome, substituindo o caractere ‘A’ por ‘*’.

**Listagem 12**: Usando replace

```
select matricula, nome, replace(nome,'A','*') from pcempr;
```

***Obs**.: Podem ser substituídos 1 caracteree por vários, vários caracterees por 1, vários por vários, ou até mesmo 1 ou vários por nenhum caracteree. Para substituir por nenhum caractere, basta abrir e fechar as aspas simples, não informando nenhum valor.*

#### ROUND

A função ROUND tem como objetivo efetuar o arredondamento de números para a quantidade de casas decimais determinada na função.

O código abaixo seleciona dos registros da tabela pctabpr os campos codprod, numregiao, ptabela, e exibe, logo em seguida, o campo ptabela com seu valor arredondado para 2 casas decimais.

**Listagem 13**: Usando Round

```
select codprod, numregiao, ptabela, roundO(ptabela,2) from pctabpr;
```

#### TRUNC

A função TRUNC tem como objetivo efetuar o truncamento(corte) de números para a quantidade de casas determinada na função.

O código abaixo seleciona dos registros da tabela pctabpr os campos codprod, numregiao, ptabela, e exibe, logo em seguida, o campo ptabela com seu valor truncado na segunda casa decimal.

**Listagem 14**: Usando função Trunc

```
select codprod, numregiao, ptabela, trunc(ptabela,2) from pctabpr;
```

#### MOD

A função MOD tem como objetivo exibir o resto da divisão de um valor por outro.

O código abaixo seleciona dos registros ds tabela pcprest, o resto da divisão do campo valor por 2.

**Listagem 15**: Usando MOD

```
select mod(valor,2) from pcprest;
```

***Obs**.: para facilitar o entendimento desta função, recomendamos utilizar a seleção da divisão informada, com truncamento para nenhuma casa decimal.*

Exemplo: select trunc(( valor/2),0) from pcprest;

### Trabalhando com Datas

#### Função SYSDATE

O código abaixo seleciona a data do sistema.

**Listagem 16:** Selecionando data do sistema com Sysdate

```
select sysdate from dual;
```

#### Aritmética com Datas

O código abaixo seleciona a data do sistema adicionando 2 dias.

**Listagem 17:** Adicionando dias

```
select sysdate+2 from dual;
```

O código abaixo seleciona a data do sistema subtraindo 2 dias.

**Listagem 18:** Subtraindo dias

```
select sysdate-2 from dual;
```

O código abaixo seleciona dos registros da tabela pcnfsaid os campos numnota, dtsaida, dtentrega, e exibe, logo em seguida, a diferença entre os campos dtentrega e dtsaida, apelidando o campo com o nome de ‘diferenca’.

**Listagem 19:** Diferença entre datas

```
select numnota, dtsaida, dtentrega, dtentrega-dtsaida diferenca from pcnfsaid;
```

### Funções de Conversão

O código abaixo seleciona dos registros da tabela pcmov os campos dtmov e numnota e, logo em seguida, o campo numnota sendo convertido para caractere, no formato de 6 dígitos.

**Listagem 20**: NUMBER to VARCHAR

```
select dtmov, numnota, to_char(numnota,'000000') from pcmov;
```

O código abaixo seleciona dos registros da tabela pcmov os campos dtmov e numnota e, logo em seguida, o campo dtmov sendo convertido para caractere, no formato ‘dd/mm’.

**Listagem 21**: DATE to VARCHAR

```
select dtmov, numnota, to_char(dtmov,'dd/mm') from pcmov;
```

**Elementos para formatação de data**

- YYYY é ano completo em números
- YEAR é ano escrito por extenso
- MM é mês em dois dígitos
- MONTH é mês escrito por extenso
- MON é mês escrito com as 3 primeiras letras iniciais
- DY é dia escrito com a abreviação em 3 letras
- DAY é dia escrito por extenso
- DD é dia em dois dígitos
- D é dia da semana O código abaixo de 1 a 7.
- DDD é dia do ano O código abaixo de 1 a 356.
- WW é número da semana dentro do ano
- W é número da semana dentro do mês

### Funções de Grupo (AVG, COUNT, MAX, MIN, SUM)

#### AVG

A função AVG tem como finalidade efetuar o cálculo de média simples do campo informado na função. O código abaixo seleciona a média dos valores do campo vltotal dos registros da tabela pcnfsaid.

**Listagem 22**: Usando função AVG

```
select avg(vltotal) from pcnfsaid;
```

#### COUNT

A função COUNT tem como finalidade efetuar a contagem do campo informado na função.

O código abaixo efetua contagem do campo numnota dos registros da tabela pcnfsaid.

**Listagem 23**: Usando função Count

```
select count(numnota) from pcnfsaid;
```

#### MAX

A função MAX tem como finalidade exibir o maior valor encontrado no campo informado na função.

O código abaixo seleciona o maior valor encontrado no campo vltotal dentre todos os registros da tabela pcnfsaid.

**Listagem 24**: Usando função MAX

```
select max(vltotal) from pcnfsaid;
```

#### MIN

Assim como acontece na função MAX, a função MIN faz exatamente o inverso, ou seja, exibe o menor valor encontrado no campo informado na função.

O código abaixo seleciona o menor valor encontrado no campo vltotal dentre todos os registros da tabela pcnfsaid.

**Listagem 25**: Usando função MIN

```
select min(vltotal) from pcnfsaid;
```

#### SUM

A função SUM tem como finalidade efetuar a soma dos valores do campo informado na função.

O código abaixo seleciona a soma dos valores do campo vltotal de todos os registros da tabela pcnfsaid.

**Listagem 26**: Usando função SUM

```
select sum(vltotal) from pcnfsaid;
```

#### GROUP BY

Sempre que algumas das funções de grupo citadas no item 6 for utilizada, e também forem selecionados campos ‘puros’ das tabelas utilizadas, é necessário utilizar a expressão ‘GROUP BY’ para determinar o agrupamento das informações que serão exibidas.

O código abaixo seleciona dos registros da tabela pcmov os campos dtmov, codprod, a soma do campo qt, onde o campo codprod for 0 ou 1, agrupando as informações pelos campos codprod e dtmov.

**Listagem 27:** Usando Group By

```
select dtmov, codprod, sum( qt)
from pcmov where codprod in (0,1)
group by codprod, dtmov;
```

### Relacionamento entre tabelas

Em boa parte das pesquisas de dados, pode ser necessário exibir informações de campos existentes em mais de uma tabela. Para que isso seja possível, é necessário determinar a junção/relacionamento que será empregada nas tabelas que estão sendo utilizadas.

O código abaixo seleciona os campos codcli[pcnfsaid], cliente[pcclient] e numnota[pcnfsaid], dos registros das tabelas pcnfsaid e pcclient, onde o valor do campo codcli da tabela pcnfsaid for igual ao valor do campo codcli da tabela pcclient. Os resultados serão exibidos apenas se essa condição for obedecida.

**Listagem 28**: Relacionamento entre tabelas

```
select pcnfsaid.codcli, pcclient.cliente, pcnfsaid.numnota
from pcnfsaid, pcclient
where pcnfsaid.codcli = pccleint.codcli;
```

O código abaixo seleciona os campos codcli[pcnfsaid], cliente[pcclient] e numnota[pcnfsaid], dos registros das tabelas pcnfsaid e pcclient, onde o valor do campo codcli da tabela pcnfsaid for igual ao valor do campo codcli da tabela pcclient. Caso os valores dos campos não sejam iguais, as informações solicitadas da tabela pcnfsaid serão exibidas, e os campos da tabela pcclient serão exibidos em branco.

**Listagem 29:** Exibindo resultados apenas se a condição for obedecida

```
select pcnfsaid.codcli, pcclient.cliente, pcnfsaid.numnota
from pcnfsaid, pcclient
where codcli = codcli (+);
```

O código abaixo seleciona os campos codusur[pcnfsaid], nome[pcusuari], codsupervisor[pcusuari] e nome[pcsuperv], dos registros das tabelas pcnfsaid, pcusuari e pcsuperv, onde o valor do campo codusur da tabela pcnfsaid for igual ao valor do campo codusur da tabela pcusuari, e o valor do campo codsupervisor da tabela pcusuari for igual ao valor do campo codsupervisor da tabela pcsuperv. Os resultados serão exibidos apenas se ambas as condições forem obedecidas.

**Listagem 30**: Exibindo campos em branco

```
select pcnfsaid.codusur, pcusuari.nome, pcusuari.codsupervisor, pcsuperv.nome
from pcnfsaid, pcusuari, pcsuperv
where pcnfsaid.codusur = pcusuari.codusur
and pcusuari.codsupervisor = pcsuperv.codsupervisor;
```

O código abaixo seleciona os campos codusur[pcnfsaid], nome[pcusuari], codsupervisor[pcusuari] e nome[pcsuperv], dos registros das tabelas pcnfsaid, pcusuari e pcsuperv, onde o valor do campo codusur da tabela pcnfsaid for igual ao valor do campo codusur da tabela pcusuari, e o valor do campo codsupervisor da tabela pcusuari for igual ao valor do campo codsupervisor da tabela pcsuperv. Caso os valores dos campos codsupervisor[pcusuari] e codsupervisor[pcsuperv] não forem iguais, as demais informações serão exibidas normalmente, porém, as informações da tabela pcsuperv não serão exibidas.

**Listagem 31**: Exemplo de relacionamento

```
select pcnfsaid.codusur, pcusuari.nome, pcusuari.codsupervisor, pcsuperv.nome
from pcnfsaid, pcusuari, pcsuperv
where pcnfsaid.codusur = pcusuari.codusur
and pcusuari.codsupervisor = pcsuperv.codsupervisor( +);
```

### Cláusula HAVING

A cláusula HAVING tem funcionamento muito parecido ao funcionamento das cláusulas de restrição WHERE, porém, com uma grande diferença. Enquanto a cláusula WHERE é utilizada para efetuar restrições de informações baseadas em campos das tabelas, a cláusula HAVING é utilizada para efetuar restrições de informações baseadas em resultados das funções de grupo (SUM, AVG, MAX, MIN e COUNT).

O código abaixo seleciona dos registros da tabela pcnfsaid os campos codcli e a soma do campo vltotal, agrupando as informações pelo campo codcli.

As informações só serão exibidas se o valor da soma do campo vltotal for maior que 10000.

**Listagem 32**: Usando Having

```
select codcli, sum(vltotal)
from pcnfsaid
group by codcli
having sum(vltotal) > 10000;
```

### Sub-select’s

Como o próprio nome diz, sub-select’s são select’s que podem ser utilizados dentro de um select maior, com o objetivo de auxiliar na extração de dados, seja através da consulta de uma nova informação, ou através da utilização de seus resultados no processo de restrição de dados.

O código abaixo seleciona todos os campos dos registros da tabela pcprodut, onde o valor do campo codprod não existir na lista distinta de valores do campo codprod de todos os registros da tabela pcmov.

**Listagem 33**: Usando sub-selects

```
select * from pcprodut
where codprod not in (select distinct(codprod) from pcmov);
```

### Conclusão

Esses foram mais alguns comandos importantíssimos no desenvolvimento de profissionais que vão atuar como programadores ou administradores de banco de dados.

Espero que tenham gostado e até a próxima.

### Veja também

- [Cálculos avançados em SQL - Revista SQL Magazine 104](http://www.devmedia.com.br/calculos-avancados-em-sql-revista-sql-magazine-104/25969)
- [Escrevendo queries otimizadas - SQL Magazine 84](http://www.devmedia.com.br/escrevendo-queries-otimizadas-sql-magazine-84/19068)