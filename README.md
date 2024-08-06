# Previsão de Estoque Inteligente na AWS com [SageMaker Canvas](https://aws.amazon.com/pt/sagemaker/canvas/)

Esta análise é referente ao desafio de projeto "Previsão de Estoque Inteligente na AWS com SageMaker Canvas" organizado e disponibilizado na plataforma da Digital Innovation One - DIO. O curso consiste em primeiros passos para realizar previsões de estoque com Machine Learning (ML) utilizando AWS SageMaker Canvas.

## Sobre o dataset

O dataset consiste em:
- ID_PRODUTO: número de identificação do produto
- DATA_EVENTO: data de referência
- PRECO: valor do preço referente ao produto
- FLAG_PROMOCAO: booleano onde 1 - está em promoção, 0 - não está em promoção
- QUANTIDADE_ESTOQUE: quantidade de unidades do produto em estoque

## Desenvolvimento do Projeto

### 1. Dataset e Feature Engineering

- Para que fosse utilizado, o dataset disponibilizado pela DIO foi importado manualmente para o SageMaker Canvas
- Os Missing Values da coluna QUANTIDADE_ESTOQUE foram substituídos por 0, enquanto os da coluna PRECO foram substituídos pela mediana. Esta funcionalidade está disponível no SageMaker Canvas, onde podemos escolher, por exemplo, o menor valor, o maior valor, a média, entre outros.

### 2. Construção e Treinamento

- Como o objetivo do modelo é prever estoque, então target feature é a QUANTIDADE_ESTOQUE. As colunas utilizadas para a previsão foram PRECO e FLAG_PROMOCAO
- O treinamento do modelo foi feito para a previsão de estoque de 1 dia após o dia mais recente presente no dataset. Ou seja, a data mais recente é 08/02/2024, então a previsão será para o dia 09/02/2024.

### 3. Análise

- Após o treinamento, o Canvas mostrou que a variável PRECO tem 9,61% de impacto sobre o estoque, enquanto a FLAG_PROMOCAO apresentou 0%. Ou seja, o estado de estar em promoção não altera de forma significativa o estoque para este grupo de produtos. Ao mesmo tmepo, o preço também não possui grande impacto.
- Sobre as métricas, temos a Average Weighted Quantile Loss de 0,06. Esta métrica avalia a predição pela média da acurácia em quartis específicos, onde quanto menor o seu valor, maior a acurácia do modelo. A Mean Absolute Percent Error (MAPE) é de 0,148. Esta métrica se refere à porcentagem da média do erro em todos os pontos, onde quanto mais próximo de 0, menos erros. A Weighted Absolute Percent Error é de 0,100, onde ela avalia o desvio geral entre os valores preditos e os observados e quanto mais próximo de 0, menos erros. A Root Mean Square Error foi de 5,765, onde quanto mais próximo de 0, menos erros. Por fim a Mean Absolute Scaled Error foi de 0,301, em que ela representa a média do erro absoluto normalizado, sendo que MASE < 1 apresenta um modelo melhor que o a tendência e MASE > 1 apresenta um modelo pior que o tendência.

### 4. Resultados

- De forma geral, foi possível observar que a maioria dos produtos observados ficaram com a sua linha de tendência (LD) quase equidistante entre a reta de cenário de descrescimento de estoque (CDE) e a de cenário de crescimento de estoque (CCE). A exemplo, temos o Produto 1000 com Demanda inicial (DI) = 24,  LD = 15, CDE 10 e CCE = 21. 
- Poucos casos é como a do Produto 1012, onde a DI = 84, LD = 70, CDE = 66 e CCE = 80, onde a linha de tendência e a de decrescimento do estoque se aproximam. Neste caso, podemos observar que se trata de um produto com maior demanda, o que é um ponto de alerta de atenção para manutenção do estoque da loja.  
