# Avaliação de aprovação de cartão de crédito com Machine Learning

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
3. [Execução do projeto](#transforação-dos-dados)
   * [1. Análise exploratória dos dados](#transforação-dos-dados)
   * [2. Transformação dos dados](#transforação-dos-dados)
   * [3. Desenvolvimento do modelo]

## Dataset

### Files:

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
 
## Transforação dos dados
 
Verifiquei que o número total de IDs únicos em *aplication_records.csv* (438510) divergia do número total de linhas (438556), nesse caso foi necessário eliminar dados duplicados. Optei por manter somente o último dado lançado.
 
Em *aplication_records.csv* a coluna `OCCUPATION_TYPE` apresentou um elevado número de dados nulos e portanto a mesma foi eliminada do dataframe para que não interferisse nos resultados do modelo.
 
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/null_2.PNG" width=600>
</p>

Em *credit_records.csv* verifiquei que não haviam dados nulos.
 
<p align="center">
  <img src="https://github.com/dougpcorrea/data_science/blob/main/1.%20Credit%20card%20aproval%20rating/images/null_3.PNG" width=600>
</p>

 
