<h1 align='center'>Avaliação de aprovação de cartão de crédito com Machine Learning</h1>

<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/credit_card.png" width=350>
</p>

Modelo de aprendizado de máquina capaz de prever se um pedido de cartão de crédito deve ser aprovado ou não com base em várias características do solicitante.

## Problema de negócio

A avaliação de aprovação de crédito é uma etapa importante para as instituições financeiras antes de concederem empréstimos ou financiamentos a um indivíduo ou empresa, ela serve para avaliar o risco envolvido e garantir que o dinheiro emprestado será recuperado com juros, o que é importante para o retorno financeiro da instituição.

Além disso, a avaliação de crédito ajuda a proteger os clientes de si mesmos, evitando que contraiam empréstimos que não possam pagar. Isso pode ajudar a evitar a possibilidade de inadimplência e problemas financeiros que podem afetar negativamente o solicitante e sua família.

E se pudessemos prever, a partir de suas características pessoais e informações profissionais, se um indivíduo, ao receber um cartão crédito, será capaz de cumprir com os pagamentos ou se tornará inadimplente?

Com os algoritmos de aprendizado de máquina podemos analisar grandes quantidades de dados e identificar padrões que os humanos podem não ser capazes de identificar. Isso ajuda as instituições financeiras a fazerem uma avaliação mais precisa do risco de emprestar dinheiro a um indivíduo ou empresa, tendo assim uma assertividade maior na hora de indentificar possíveis inadimplências.

E-mail: dougpcorrea@hotmail.com <br>
Celular: [+55 (51) 98492 5343](https://wa.me/5551984925343)

## Sumário
 
1. [Dataset](#Dataset)
2. [Tecnologias utilizadas](#tecnologias-utilizadas)
3. [Execução do projeto](#análise-exploratória-e-transformação-dos-dados)
   * [1. Análise exploratória dos dados](#análise-exploratória-e-transformação-dos-dados)
   * [2. Desenvolvimento do modelo](#desenvolvimento-do-modelo)

## Dataset

### Files:

Você pode acessar os dados utilizados neste modelo clicando [aqui](www.google.com)<br>
Os dados são públicos e foram disponibilizados gratuitamente na plataforma [Kaggle](https://www.kaggle.com/datasets/rikdifos/credit-card-approval-prediction) para fins educacionais.

<li> application_records.csv → Informações e características dos aplicantes <br>
<li> credit_records.csv → Histórico mensal de situação de inadimplência dos aplicantes <br>

#### aplication_records.csv

`ID` → ID do aplicante <br>
`CODE_GENDER` → Gênero <br>
`FLAG_OWN_CAR` → Possui veículo próprio <br>
`FLAG_OWN_REALTY` → Possui imóvel próprio <br>
`CNT_CHILDREN` → Quantide de filhos <br>
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
 
Em *aplication_records.csv* a coluna `OCCUPATION_TYPE` apresentou um elevado número de dados nulos e portanto foi necessário eliminá-la do dataframe. Dados nulos representam uma falta de informação sobre as instâncias usadas para treinar o modelo de aprendizado de máquina. Quando um modelo é treinado com dados incompletos pode haver uma perda de precisão e confiabilidade nos resultados produzidos pelo mesmo, se houvesse por exemplo uma correlação entre os dados nulos e os resultados que estamos tentando prever isso poderia causar um viés no modelo.

<h5 align="center">Dados nulos em *aplication_records.csv*</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/null_2.PNG" width=500>
</p>

Em *credit_records.csv* verifiquei que não haviam dados nulos.

<h5 align="center">Dados nulos em *credit_records.csv*</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/null_3.PNG" width=500>
</p>

Transformei todos os dados não numéricos em numéricos (por exemplo "Y" e "N" tornaram-se 1 e 0) e fiz uma verificação nas colunas de características dos aplicantes e verifiquei que haviam alguns *outliers*.

<h5 align="center">Distribuição dos dados em *application_records.csv*</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/outliers_1.PNG" width=950>
</p>

Eliminei estes outliers e agora os dados ficaram mais homogêneos. Outliers podem ser vistos como valores extremos e podem ter um impacto significativo na precisão geral do modelo. Eles podem fazer com que o modelo superajuste os dados de treinamento e resulte em generalização ruim para novos dados, eles podem distorcer os resultados do modelo puxando os valores previstos para eles. Isso pode resultar em um modelo tendencioso que não reflete com precisão os dados subjacentes.

<h5 align="center">Distribuição dos dados em *application_records.csv* após eliminação de outliers</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/outliers_2.PNG" width=950>
</p>

#### Definiação da regra de negócio
 
Os casos que tiveram histórico de inadimplência de mais de 60 dias ao menos uma vez serão considerados como perda/prejuízo e portanto são futuros casos como estes que tentaremos prever.
 
Em *credit_records.csv*, mantive apenas um registro para cada ID, constando o máximo de dias que o aplicante esteve em atraso em seu histórico, e na coluna `STATUS` defini como esta regra 0 e 1, sendo 0 para nenhum atraso de mais de 60 dias e 1 para pelo menos um atraso de mais de 60 dias em seu histórico.
 
Juntei *application_records.csv* e *credit_records.csv* em um dataframe final.
 
Depois que o modelo ficou pronto testei outros prazos como regra de corte, mas decidi manter em 60 dias pois esse modelo apresentou uma porcentagem de assertividade levemente maior.
 
<h5 align="center">Balanceamento dos dados em 'STATUS' no dataframe final</h5>
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/balance.PNG" width=400>
</p>
 
Verifiquei que há um expressivo desbalanceamento nos status dos aplicantes, isso precisará ser abordado novamente no desenvolvimento do modelo.
 
## Desenvolvimento do modelo
 
