<h1 align='center'>Avaliação de aprovação de cartão de crédito com Machine Learning</h1>

<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/credit_card.png" width=350>
</p>

Modelo de aprendizado de máquina capaz de prever se um pedido de cartão de crédito deve ser aprovado ou não com base em várias características do solicitante.

O notebook do projeto pode ser acessado clicando [aqui](https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/credit_card_approval_rating.ipynb)

## Problema de negócio

A avaliação de aprovação de crédito é uma etapa importante para as instituições financeiras antes de concederem empréstimos ou financiamentos a um indivíduo ou empresa, ela serve para avaliar o risco envolvido e garantir que o dinheiro emprestado será recuperado com juros, o que é importante para o retorno financeiro da instituição.

Além disso, a avaliação de crédito ajuda a proteger os clientes de si mesmos, evitando que contraiam empréstimos que não possam pagar. Isso pode ajudar a evitar a possibilidade de inadimplência e problemas financeiros que podem afetar negativamente o solicitante e sua família.

E se pudéssemos prever, a partir de suas características pessoais e informações profissionais, se um indivíduo, ao receber um cartão crédito, será capaz de cumprir com os pagamentos ou se tornará inadimplente?

Com os algoritmos de aprendizado de máquina podemos analisar grandes quantidades de dados e identificar padrões que os humanos podem não ser capazes de identificar. Isso ajuda as instituições financeiras a fazerem uma avaliação mais precisa do risco de emprestar dinheiro a um indivíduo ou empresa, tendo assim uma assertividade maior na hora de identificar possíveis inadimplências.

E-mail: dougpcorrea@hotmail.com <br>
Celular: [+55 (51) 98492 5343](https://wa.me/5551984925343)

## Sumário
 
1. [Dataset](#Dataset)
2. [Tecnologias utilizadas](#tecnologias-utilizadas)
3. [Execução do projeto](#análise-exploratória-e-transformação-dos-dados)
4. [Análise exploratória e transformação dos dados](#análise-exploratória-e-transformação-dos-dados)
5. [Desenvolvimento do modelo](#desenvolvimento-do-modelo)

## Dataset

#### Arquivos:

Você pode acessar os arquivos que contém os dados utilizados neste modelo clicando [aqui](https://www.kaggle.com/datasets/rikdifos/credit-card-approval-prediction)

<li> application_records.csv → Informações e características dos aplicadores <br>
<li> credit_records.csv → Histórico mensal de situação de inadimplência dos aplicadores <br>

#### aplication_records.csv

`ID` → ID do aplicante <br>
`CODE_GENDER` → Gênero <br>
`FLAG_OWN_CAR` → Possui veículo próprio <br>
`FLAG_OWN_REALTY` → Possui imóvel próprio <br>
`CNT_CHILDREN` → Quantidade de filhos <br>
`AMT_INCOME_TOTAL` → Renda total <br>
`NAME_INCOME_TYPE` → Origem da renda <br>
`NAME_EDUCATION_TYPE` → Nível de educação <br>
`NAME_FAMILY_STATUS` → Estado civil <br>
`NAME_HOUSING_TYPE` → Tipo de moradia <br>
`DAYS_BIRTH` → Contagem regressiva de dias até a data de nascimento (-1 corresponde a ontem) <br>
`DAYS_EMPLOYED` → Contagem regressiva de dias até a data de contrato. (+1 desemprego há 1 dia, -1 contrato ontem) <br>
`FLAG_MOBIL` → Possui telefone celular <br>
`FLAG_WORK_PHONE` → Possui telefone celular corporativo <br>
`FLAG_PHONE` → Possui telefone residencial <br>
`FLAG_EMAIL` → Possui e-mail <br>
`OCCUPATION_TYPE` → Profissão <br>
`CNT_FAM_MEMBERS` → Número de membros na família <br>

Total de entradas: 438556

#### credit_records.csv

Cada ID possui uma entrada para cada mês desde sua aplicação, contendo na coluna 'STATUS' uma flag que corresponde a um determinado <br>
intervalo de dias que se passaram desde a contratação do crédito sem que houvesse pagamento

`ID` → ID do aplicante <br>
`MONTHS_BALANCE` → Contagem regressiva de meses desde a aplicação (-1 corresponde ao mês passado) <br>
`STATUS` → <br>
    <li> 0: 1-29 dias <br>
    <li> 1: 30-59 dias <br>
    <li> 2: 60-89 dias <br>
    <li> 3: 90-119 dias <br>
    <li> 4: 120-149 dias <br>
    <li> 5: Mais de 150 dias de inadimplência <br>
    <li> C: Pago naquele mês <br>
    <li> X: Sem debitos naquele mês <br>

Total de entradas: 1048574
     
## Tecnologias utilizadas

<li> Python 
<li> Pandas
<li> Numpy
<li> Matplotlib
<li> Seaborn
<li> Scikit Learn
<li> Imblearn
<li> XGBoost
 
## Análise exploratória e transformação dos dados
 
#### Saneamento dos dados
 
Verifiquei que o número total de IDs únicos em *aplication_records.csv* (438510) divergia do número total de linhas (438556), nesse caso foi necessário eliminar dados duplicados. Optei por manter somente o último dado lançado.
 
Em *aplication_records.csv* a coluna `OCCUPATION_TYPE` apresentou um elevado número de dados nulos e, portanto, foi necessário eliminá-la do dataframe. Dados nulos representam uma falta de informação sobre as instâncias usadas para treinar o modelo de aprendizado de máquina. Quando um modelo é treinado com dados incompletos pode haver uma perda de precisão e confiabilidade nos resultados produzidos pelo mesmo, se houvesse por exemplo uma correlação entre os dados nulos e os resultados que estamos tentando prever isso poderia causar um viés no modelo.

<h5 align="center">Dados nulos em *aplication_records.csv*</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/null_2.PNG" width=500>
</p>

Em *credit_records.csv* verifiquei que não haviam dados nulos.

<h5 align="center">Dados nulos em *credit_records.csv*</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/null_3.PNG" width=500>
</p>

Transformei todos os dados não numéricos em numéricos (por exemplo "Y" e "N" tornaram-se 1 e 0) e fiz uma verificação nas colunas de características dos aplicadores e verifiquei que haviam alguns *outliers*.

<h5 align="center">Distribuição dos dados em *application_records.csv*</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/outliers_1.PNG" width=950>
</p>

Eliminei estes outliers e agora os dados ficaram mais homogêneos. Outliers podem ser vistos como valores extremos e podem ter um impacto significativo na precisão geral do modelo. Eles podem fazer com que o modelo superajuste os dados de treinamento e resulte em generalização ruim para novos dados, eles podem distorcer os resultados do modelo puxando os valores previstos para eles. Isso pode resultar em um modelo tendencioso que não reflete com precisão os dados subjacentes.

<h5 align="center">Distribuição dos dados em *application_records.csv* após eliminação de outliers</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/outliers_2.PNG" width=950>
</p>

#### Definição da regra de negócio
 
Os casos que tiveram histórico de inadimplência de mais de 60 dias ao menos uma vez serão considerados como perda/prejuízo e, portanto, são futuros casos como estes que tentaremos prever.
 
Em *credit_records.csv*, mantive apenas um registro para cada ID, constando o máximo de dias que o aplicante esteve em atraso em seu histórico, e na coluna `STATUS` defini como esta regra 0 e 1, sendo 0 para nenhum atraso de mais de 60 dias e 1 para pelo menos um atraso de mais de 60 dias em seu histórico.
 
Juntei *application_records.csv* e *credit_records.csv* em um dataframe final.
 
Depois que o modelo ficou pronto testei outros prazos como regra de corte, mas decidi manter em 60 dias pois esse modelo apresentou uma porcentagem de assertividade levemente maior.
 
<h5 align="center">Balanceamento dos dados em 'STATUS' no dataframe final</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/balance.PNG" width=400>
</p>
 
Verifiquei que há um expressivo desbalanceamento nos status dos aplicadores, isso precisará ser abordado novamente no desenvolvimento do modelo.
 
## Desenvolvimento do modelo
  
Dividi os dados entre em eixos *x* e *y*, sendo *x* para classes categóricas e *y* para variáveis dependentes, e depois dividi os dados para treino e para teste do modelo.

#### Dimensionamento e balanceamento
  
Dimensionei os dados das colunas de classificação em um intervalo entre 0 e 1. Este escalonamento ajuda a normalizar os dados, evitando que um conjunto de dados com variâncias muito diferentes tenha impacto no desempenho do modelo. Por exemplo, se você tiver um conjunto de dados em que as variáveis possuam diferentes unidades de medida (por exemplo, altura em centímetros e peso em quilos), os dados não estarão em uma escala comparável e isso pode afetar negativamente o desempenho do modelo. 

Devido ao expressivo desbalanceamento no status dos aplicadores, apliquei uma técnica para aumentar artificialmente a quantidade de amostras da classe minoritária para equilibrar a distribuição das classes no conjunto de dados. Utilizei a técnica SMOTE (Synthetic Minority Over-sampling Technique), que cria novas amostras sintéticas interpolando entre as amostras existentes.
  
#### Definindo modelos e testando acurácias

Elenquei alguns modelos que se aplicam na solução de problemas de classificação binária e testei suas acurácias.

<h5 align="center">Resultados de acurácia dos modelos testados</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/results.PNG" width=900>
</p>
  
O modelo que obteve melhor acurácia foi o XGBoost, obtendo um score de aproximadamente 95%.
  
O XGBoost (Extreme Gradient Boosting) é um algoritmo supervisionado baseado em árvore que usa técnicas de regularizaçã e gradient boosting para aumentar a precisão do modelo.
  
<h5 align="center">Resultados de precisão do modelo escolhido</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/results_2.PNG" width=450>
</p>

## Conclusão
 
Apesar dos dados limitados e com um volume de baixa expressão para o desenvolvimento de algoritmos complexos de aprendizado de máquina, podemos perceber uma precisão bastante positiva na predição dos resultados. 
  
#### Desenvolvimento futuro
* Desenvolver uma aplicação de formulário para automatizar o processo de avaliação de aprovação do cartão de crédito
