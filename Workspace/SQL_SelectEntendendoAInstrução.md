# SQL Select: Entendendo a instrução SELECT

## Este artigo apresentará uma introdução à instrução SELECT. Será apresentada uma série de exemplos para ilustrar e tornar o seu entendimento mais fácil.



**Por que eu devo ler este artigo:**A instrução SQL SELECT faz parte da vida dos desenvolvedores de aplicativos ou sistemas de informação. O objetivo desse artigo é apresentar a instrução SELECT e seus detalhes de uso, assim como suas melhores práticas e, desde o início, como escrevê-la de forma que tenha sempre boa performance.



A instrução SELECT está presente em todas as aplicações, em menor ou maior quantidade, de forma mais simples ou mais complexa, de forma mais ou menos eficiente, mas está sempre lá.

Portanto, se você é um desenvolvedor, você já utilizou ou utilizará essa instrução. Assim, nada melhor do que conhecer seus detalhes e como utilizá-la de maneira a tornar sua aplicação o mais eficiente possível no acesso aos seus dados.

Ver mais

Suporte ao alunoAnotarMarcar como concluídoCódigo fonte

[Artigos](https://www.devmedia.com.br/artigos/)[Banco de Dados](https://www.devmedia.com.br/artigos/banco-de-dados)SQL Select: Entendendo a instrução SELECT





A instrução SELECT é parte integrante e primordial da linguagem SQL que nasceu junto com a teoria dos bancos de dados relacionais. Para muitos profissionais atualmente no mercado, ela deve ser a única forma conhecida de se ter acesso às informações armazenadas em um SGBD.

Ela foi inicialmente definida no final da década de 70 e de lá para cá, sua utilização cresceu vertiginosamente e ela se transformou quase em um modo único de acesso a dados armazenados no SGBD.

A instrução SELECT é extremamente funcional, se utilizada corretamente, trazendo resultados agrupados, classificados, detalhados ou totalizados, de várias fontes de dados, ou tabelas, em vários formatos diferentes, sem que o desenvolvedor precise escrever uma única linha sequer de código.

Mas esta mesma instrução pode ser o vilão de graves problemas de performance na sua aplicação se não for escrita de maneira apropriada ou se o seu banco de dados não estiver preparado para executá-la da forma que você a escreveu.

Este artigo apresentará uma introdução à instrução SELECT. Você encontrará a forma de montagem dessa instrução, com o acesso a uma única tabela de um banco de dados, porém entrando em detalhes como restrições dos dados, classificação, agrupamento, sumarização, cálculo de médias, entre outros, de forma que você consiga começar a escrever as suas primeiras instruções de forma segura e simples.

Serão apresentados uma série de exemplos para ilustrar e tornar o seu entendimento mais fácil. Nosso objetivo é trazer para o leitor as melhores práticas do uso da instrução SELECT para que se possa utilizá-la no desenvolvimento das suas aplicações trazendo o melhor benefício em simplicidade no seu desenvolvimento e performance na sua utilização.

**SELECT FROM tabela x**

O princípio de tudo é a forma mais básica da instrução SELECT, utilizando a cláusula FROM. A sintaxe mais simples da instrução é:

```
SELECT
<lista de colunas> FROM <nome de uma tabela>
```

Não existe versão menor dessa instrução quando o objetivo é recuperar dados. Existem SGBDs de alguns fabricantes, onde a instrução SELECT existe sem a cláusula FROM, mas apenas para fazer um cálculo ou retornar uma constante em um bloco de instruções programáveis. Mas esse não é o nosso foco.

Vamos utilizar como exemplo uma tabela de funcionários de uma empresa hipotética que armazena as seguintes informações: matrícula, nome, superintendência, departamento e salário.

Para recuperarmos o número da matrícula e o nome de todos os funcionários, deveríamos escrever: “SELECT matrícula, nome”.

Ou ainda mais simples, para recuperarmos todas as colunas da tabela, bastaria que colocássemos um “*” no lugar do nome das colunas que, dessa forma, o SGBD entenderia que queremos todas as colunas. Mas a instrução SELECT não vive sem a cláusula FROM.

Na cláusula FROM você especifica de onde virão seus dados, que será o nome da tabela, ou das tabelas. Assim, o comando estaria completo da seguinte forma:

```
SELECT matricula, nome FROM Funcionarios
```

Esta já é uma instrução SELECT completa. Se você não disser mais nada, o SGBD trará para você todos os registros, ou linhas, de cada uma das colunas especificadas da tabela informada. Seriam apresentados para você a matrícula e nome de todos os funcionários cadastrados na tabela Funcionários.

Na **Listagem 1** você encontra os dados de exemplo da tabela de Funcionários que iremos utilizar nesse artigo.

**Listagem 1.** Lista dos dados da tabela Funcionários utilizada como exemplo nesse artigo

```
select * from funcionarios;

+-----------+----------+------------------+--------------+----------+
| matricula | nome     | superintendencia | departamento | salario  |
+-----------+----------+------------------+--------------+----------+
|       123 | Joao     | Sup A            | TI    ...
```