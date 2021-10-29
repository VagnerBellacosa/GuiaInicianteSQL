# Linguagem SQL: ORDER BY

## Esse artigo explica a utilização da cláusula ORDER BY no comando SELECT com o objetivo de ordenar os resultados de uma consulta.



Por que eu devo ler este artigo:Quando retornamos dados através de uma consulta, nem sempre a ordem em que os dados são apresentados é a ideal, pois eles podem vir misturados, gerando um trabalho adicional para ordená-los. Evitamos esse trabalho quando combinamos o comando SELECT com a cláusula ORDER BY, que ordena os dados por uma ou mais colunas da tabela. Vamos conhecer a sintaxe de ORDER BY e as opções de ordenação que podemos usar nas consultas.

Ver mais

[Voltar](https://www.devmedia.com.br/sql/order-by)Suporte ao alunoAnotarMarcar como concluídoCódigo fonte

[Artigos](https://www.devmedia.com.br/artigos/)[Banco de Dados](https://www.devmedia.com.br/artigos/banco-de-dados)Linguagem SQL: ORDER BY

Um negócio pode gerar muitos dados, que serão úteis de acordo com a forma em que são apresentados. Como exemplo, temos uma loja que registra suas vendas em uma tabela chamada **VENDAS_DIARIAS** em um banco de dados. Se observar todas as vendas de um dia na ordem em que foram gravadas pode ser difícil de entender tantos dados misturados. Esses dados precisam ser ordenados de forma a facilitar sua leitura.

Veja o resultado na **Figura 1** de uma consulta a tabela **VENDAS_DIARIAS**.

![Resultado de uma consulta a tabela VENDAS_DIARIAS](https://www.devmedia.com.br/arquivos/Artigos/40732/fig1.png)**Figura 1**. Resultado de uma consulta a tabela VENDAS_DIARIAS