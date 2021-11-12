# Subqueries: questões de concurso resolvidas

## É imprescindível que o desenvolvedor atual saiba trabalhar com subqueries, pois além de resolver problemas que não são possíveis de serem resolvidos com JOIN, o assunto é bem trabalhado em concursos públicos.

Subquery é uma instrução do tipo SELECT dentro de outra instrução SQL, possibilitando assim a realização de consultas mais complexas, que de outra forma seriam extremamente complicadas ou impossíveis de serem feitas, como por exemplo utilizando JOINS.

Este artigo tem como finalidade exemplificar a utilização de subqueries na prática, utilizando para isso exemplos de questões que foram trabalhadas em concursos públicos.

### Questão 1: Controladoria geral da união (CGU)

Na prova do concurso público da Controladoria geral da União, tivemos a seguinte questão:

Uma subquery (subconsulta) é um comando SELECT que foi "embutido" em outro comando SELECT, UPDATE, DELETE ou dentro de outra subquery. A finalidade da subquery é retornar um conjunto de linhas para a query (consulta) ou comando principal. Com relação às subqueries, é correto afirmar que:

**A.** uma subquery não precisa estar incluída entre parêntesis.

**B.** uma subquery sempre deve estar do lado esquerdo do operador de comparação.

**C.** uma subquery pode conter a cláusula ORDER BY.

**D.** o operador IN não pode ser utilizado em uma subquery que retorne múltiplas linhas.

**E.** o operador igual "=" não pode ser utilizado em uma consulta que contenha uma subquery que retorne múltiplas linhas.

A resposta correta para esta questão seria a letra **E** (o operador igual "=" não pode ser utilizado em uma consulta que contenha uma subquery que retorne múltiplas linhas)

Vamos compreender melhor porque as outras alternativas estão incorretas:

**A:** A alternativa diz: **... não precisa ...**, porém toda subquery **deve** estar incluída entre parênteses para se diferenciar da consulta principal. É recomendado a utilização de um Alias para a subquery logo após o parênteses, facilitando assim a sua identificação.

**B:** A alternativa diz: **...sempre deve estar do lado esquerdo ...\***, porém, não faz diferença para o resultado da consulta se a subquery esta a esquerda ou a direita do operador de comparação.

**C:** A Alternativa diz: **...pode conter order by\***, porém uma subquery não pode a cláusula order by. Caso seja necessário, a cláusula deve ser movida para a consulta principal.

**D:** A Alternativa diz **... não pode ser utilizado em uma subquery ...\*** porém o operador **IN** pode ser utilizado em uma subquery que retorna múltiplas linhas.

### Questão 2: Tribunal de Contas do Município de São Paulo (TCMSP)

Na prova do Tribunal de Contas do Município de São Paulo tivemos a seguinte questão:

Considere as tabelas relacionadas, e respectivas instância, mostradas na **Figura 1**.

![Estrutura de tabelas](https://arquivo.devmedia.com.br/pablo/pacotes/tLV9Bc4zvkSk7qfokppwinAjCObCPUS6jM0SDm1o.png)**Figura 1.**. Estrutura de tabelas da questão

O comando SQL apresentado na **Listagem 1** produz um resultado com apenas uma coluna, cujo(s) valor(es) é/são:



**A.** 1





**B.** 1,2,3





**C.** 4,5





**D.** 1,2,3,4,5





**E.** NULL



```
SELECT A FROM X1 WHERE
     NOT EXISTS
     (SELECT * FROM X3 WHERE
      NOT EXISTS
      (SELECT * FROM X2 WHERE
       X1.A = X2.C AND X3.B=X2.D))
```

**Listagem 1.** Comando SQL da questão 2.

A resposta correta para esta questão seria a letra **A** (1).

Esta questão foi baseada em uma query inicial, dessa forma, para entender melhor como chegamos no resultado, vamos detalhar o funcionamento da query:

Para simplificar recomenda-se analisar o comando SQL por partes, começando do mais interno e seguir filtrando as linhas. Dessa forma, a instrução SELECT mais interna é:

```
(SELECT * FROM X2 WHERE
     X1.A = X2.C AND X3.B=X2.D)
```

O resultado dessa instrução vai comparar os dados da coluna A da tabela X1 com a coluna C da tabela X2. De forma semelhante, também será feita uma comparação com os dados da tabela X3 e X2, porém entre as colunas B e D.

O candidato deve analisar linha a linha começando da tabela X1 e X2 e, em seguida, analisar o resultado da linha para os valores de X3 e X2. Após estudar cuidadosamente o resultado dessa consulta mais interna (com base na **Figura 1**)pode-se notar que ela vai gerar os dados apresentados na **Tabela 1**.

| 1    | 1    |
| ---- | ---- |
| 1    | 3    |
| 1    | 5    |
| 2    | 1    |
| 2    | 3    |
| 3    | 1    |
| 3    | 1    |

**Tabela 1.** Resultado da subquery

Com base neste resultado, executamos a subquery do meio, conforme podemos ver na **Listagem 2**.

```
SELECT * FROM X3 WHERE
    NOT EXISTS
```

**Listagem 2**. Resultado da query

<

Esta consulta vai analisar para quais valores únicos da coluna C não existem os valores 1, 3 e 5 (o conteúdo completo da tabela de X3), portanto, observando os dados da **Tabela 1** é possível notar que apenas quando a coluna C for igual a 1 termos a sequência 1, 3 e 5. Dessa forma o resultado da subconsulta é uma nova tabela em memória com os valores 2, 2, 3 e 3.

Podemos partir agora para a consulta principal, que podemos ver abaixo na **Listagem 3**.

```
SELECT A FROM X1 WHERE
   NOT EXISTS
```

**Listagem 3**. Resultado da query

Esta consula irá trazer todos os valores da coluna A que não existam no resultado da query anterior, que gerou os valores 2,2,3 e 3. Dessa forma, da coluna A da tabela X1, o único valor que não esta presente é o valor 1, que no caso, é a resposta final para a questão.

### Questão 3: Instituto Brasileiro de Geografia e Estatística (IBGE)

Na prova do concurso público do Instituto Brasileiro de Geografia e Estatística, tivemos a seguinte questão:

Observe a estrutura de tabelas ilustrado na **Figura 2**.

![Estrutura de tabelas](https://arquivo.devmedia.com.br/pablo/pacotes/A3ZtGNHbd8ZyWgNCUOhqvKmXRPmBzKh8LrwGWWLn.jpeg)**Figura 2.**. Estrutura de tabelas da questão

Com base nesta estrutura, escreva um comando SQL que responda à pergunta: quais os nomes dos empregados do departamento cujo identificador do departamento é 200 e que não tiraram férias no ano de 2000:

Podemos dar como resposta para esta questão a query vista na **Listagem 4**.

```
SELECT nomeEmpregado
  FROM Empregado
  WHERE idDepartamento = 200
  AND   idEmpregado NOT IN (SELECT idEmpregado
                            FROM Ferias WHERE ano = 2000);
```

**Listagem 4**. Query reposta da questão 3

Vamos entender melhor a query acima. Na **Linha 1** selecionamos a coluna nomeEmpregado, e logo na **Linha 2** informamos que esses dados serão tirados da tabela Empregado. Na **linha 3** adicionamos um filtro informando que somente serão trazidos resultados onde o valor da coluna idDepartamento seja igual a 200 e logo em seguida, na **linha 4** informamos que além do filtro anterior, obrigatoriamente a query deve respeiter um segundo filtro, que define que o valor da coluna idEmpregado não pode aparecer no resultado de uma subquery. A subquery referenciada, nas **Linhas 4 e 5** seleciona a coluna idEmpregado da tabela Ferias onde o valor da coluna ano é igual a 2000.

### Questão 4: Instituto Brasileiro de Geografia e Estatística (IBGE)

Ainda na prova do Instituto Brasileiro de Geografia e Estatística, temos a seguinte quetão:

Escreva um comando SQL que responda à pergunta: quais os identificadores e os nomes dos empregados que recebem salário acima da média de salário dos empregados da empresa e que possuem mais de 2 dependentes?

Podemos dar como resposta para esta questão a query vista na **Listagem 5**.

```
SELECT idEmpregado
       , nomeEmpregado
  FROM Empregado
  WHERE numeroDependentes > 2
    AND salario > (SELECT AVG(salario) FROM Empregado);
```

**Listagem 5.** Query resposta da questão 4

Vamos entender melhor a query acima. Na **Linha 1 e 2** selecionamos as colunas idEmpregado e nomeEmpregado. Na **Linha 3** informamos que esses dados serão trazidos da tabela Empregado. Na **Linha 4** definimos o primeiro item do filtro, que é o valor da coluna numeroDependentes ser maior que 2. Na **Linha 5**, adicionamos o segundo filtro obrigatório que é o valor da coluna salario ser maior que o valor de uma subquery informada. A subquery mencionada na **linha 5** é responsável por trazer a média do valor da coluna salario através da função de agregação AVG diretamente da tabela Empregado.

Para mais informações, temos aqui na DevMedia um curso exclusivo sobre este tema! [Avançando com Subqueries](https://www.devmedia.com.br/curso/avancando-com-subqueries/2231)

[Saiba maisVeja a Série **SQL nível Jedi: Subqueries**
  ](https://www.devmedia.com.br/sql-queries/)