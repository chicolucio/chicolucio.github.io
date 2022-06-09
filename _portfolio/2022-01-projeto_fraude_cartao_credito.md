---
name: Credit card fraud detection
title: Detecção de fraudes em cartões de crédito
image: /portfolio/projeto_fraude_cartao_credito_files/fraud_logo.jpg
position: 2
period: January 2022
toc: true
toc_min_header: 2
toc_max_header: 3
mathjax: true
header:
  image: https://raw.githubusercontent.com/chicolucio/credit-card-fraud-detection/master/img/fraud.jpg
description: |
  In this work, we will use a credit card transactions database to study 
  different algorithms, which can be used to classify a given transaction 
  as fraudulent or not. We will see how to deal with unbalanced databases
  in Scikit Learn.
---

## Abstract

In this work, we will use a credit card transactions database to study 
different algorithms, which can be used to classify a given transaction 
as fraudulent or not. We will see how to deal with unbalanced databases
in Scikit Learn.

The following page is written in Brazilian Portuguese.

## Resumo

Nas últimas décadas, com o aumento constante de operações financeiras
realizadas, fraudes com cartões de crédito têm se tornado um problema
considerável. Grande poder computacional é direcionado para tentar detectar
operações fraudulentas, especialmente com o uso cada vez mais intensivo de
bancos online e e-commerce. Diversas estratégias de Data Mining e Machine
Learning vêm sendo empregadas neste contexto.

Neste trabalho, utilizaremos uma base com dados de transações feitas com cartões
de crédito e estudaremos diferentes algoritmos que podem ser utilizados para
classificar uma determinada transação como fraudulenta ou não.

## Contextualização

<center>
    <img alt="hacked" src="https://raw.githubusercontent.com/chicolucio/credit-card-fraud-detection/master/img/hacked.jpg">
</center>

### Breve descrição de um sistema de detecção de fraudes

Muitos tipos de fraudes podem ser realizados, desde omissão de fontes de renda,
passando por lavagem de dinheiro, até clonagem de cartão. Na tentativa de se
prevenir, a indústria financeira faz uso dos chamados *Sistemas Baseados em
Regras*, também chamados de *Sistemas de Produção*, que armazenam e manipulam
dados buscando inferir se uma determinada operação é fraudulenta ou não.
Atualmente, tais sistemas fazem uso de ferramentas de machine learning.

Podemos imaginar um sistema de detecção de fraude em um banco como a figura a
seguir. Há o banco de dados da instituição sobre o qual são realizados
treinamentos de algoritmos gerando modelos que representam as características
das transações armazenadas.

<a href="https://linktr.ee/flsbustamante"><img src="https://raw.githubusercontent.com/chicolucio/credit-card-fraud-detection/master/img/fraud_flux.png" alt="fraud flux"></a>

<center>    
    <p style="font-size:80%;color:gray">Fluxograma de um Sistema de Detecção de Fraudes. Adaptado <a href="https://doi.org/10.22937/IJCSNS.2021.21.9.4">da literatura.</a></p>
</center>

O modelo, então, é usado para decidir se novas transações serão aceitas como
genuínas ou recusadas como fraudulentas. Uma transação aceita será executada e
adicionada ao banco de dados para melhorar o modelo. Uma recusada, será
destinada para avaliação manual. Se for considerada genuína, será executada e
adicionada à base de dados como tal. Caso se confirme a fraude, será confirmada
a rejeição e registrada na base como efetivamente fraude.

Como vemos, parte crucial do processo é treinar um modelo que seja eficaz. Um
modelo bem desenvolvido identificaria não apenas fraudes mas, também,
determinaria a probabilidade de um comportamento fraudulento.

### Base de dados utilizada neste estudo

O dataset utilizado neste trabalho pode ser baixado [deste link do
Kaggle](https://www.kaggle.com/mlg-ulb/creditcardfraud). Trata-se de uma base de
dados com transações de cartões de crédito registradas por operadoras europeias
em dois dias do mês de setembro de 2013. São 284.807 transações das quais apenas
492 foram registradas como fraudes. Observe a quantidade elevada de transações
considerando que são apenas de dois dias (e de 8 anos atrás, imagine quanto deve
ser o volume atualmente para um mesmo intervalo de tempo) e como é uma base de
dados muito desbalanceada, já que somente 0,172 % do total de registros são
fraudulentos.

Na próxima seção, faremos uma melhor descrição do dataset e começaremos a
entender como treinar um modelo eficaz com tal desbalanceamento.

Neste artigo, apenas os resultados serão mostrados. Caso queira ver o código
completo, [visite o repositório do
projeto](https://github.com/chicolucio/credit-card-fraud-detection).

## Bibliotecas utilizadas e uma primeira visão do dataset

Para facilitar a reprodução do presente estudo, segue uma lista das bibliotecas
utilizadas e suas versões:

|      Biblioteca      |   Versão   |
| -------------------- | ---------- |
| Pandas               |      1.3.3 |
| Matplotlib           |      3.4.2 |
| Seaborn              |     0.11.2 |
| Numpy                |     1.20.3 |
| Scikit-Learn         |      1.0.1 |
| Imbalanced-Learn     |      0.8.1 |
    

A versão do Python utilizada foi a 3.9.7.

O pacote [Pandas](https://pandas.pydata.org/) foi utilizado para trabalhar com
os dados e construir matrizes de correlações. Os pacotes
[Matplotlib](https://matplotlib.org/) e [Seaborn](https://seaborn.pydata.org/)
foram utilizados para construir os gráficos. O [NumPy](https://numpy.org/) foi
utilizado no desenvolvimento de algumas funções para tratamentos matemáticos.
O [Scikit-Learn](https://scikit-learn.org/stable/) possibilitou o uso de
algoritmos de classificação e previsão.

As cinco bibliotecas descritas acima são bem conhecidas.
A biblioteca [Imbalanced Learn](https://imbalanced-learn.org/stable/), como o
nome indica, é construída sobre o Scikit-Learn com o intuito de
lidar melhor com datasets desbalanceados, como o deste estudo. Foi utilizada em
combinação com o Scikit-Learn padrão.

A seguir, vemos as 5 primeiras entradas da base de dados:

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Time</th>
      <th>V1</th>
      <th>V2</th>
      <th>V3</th>
      <th>V4</th>
      <th>V5</th>
      <th>V6</th>
      <th>V7</th>
      <th>V8</th>
      <th>V9</th>
      <th>...</th>
      <th>V21</th>
      <th>V22</th>
      <th>V23</th>
      <th>V24</th>
      <th>V25</th>
      <th>V26</th>
      <th>V27</th>
      <th>V28</th>
      <th>Amount</th>
      <th>Class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>-1.359807</td>
      <td>-0.072781</td>
      <td>2.536347</td>
      <td>1.378155</td>
      <td>-0.338321</td>
      <td>0.462388</td>
      <td>0.239599</td>
      <td>0.098698</td>
      <td>0.363787</td>
      <td>...</td>
      <td>-0.018307</td>
      <td>0.277838</td>
      <td>-0.110474</td>
      <td>0.066928</td>
      <td>0.128539</td>
      <td>-0.189115</td>
      <td>0.133558</td>
      <td>-0.021053</td>
      <td>149.62</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>1.191857</td>
      <td>0.266151</td>
      <td>0.166480</td>
      <td>0.448154</td>
      <td>0.060018</td>
      <td>-0.082361</td>
      <td>-0.078803</td>
      <td>0.085102</td>
      <td>-0.255425</td>
      <td>...</td>
      <td>-0.225775</td>
      <td>-0.638672</td>
      <td>0.101288</td>
      <td>-0.339846</td>
      <td>0.167170</td>
      <td>0.125895</td>
      <td>-0.008983</td>
      <td>0.014724</td>
      <td>2.69</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.0</td>
      <td>-1.358354</td>
      <td>-1.340163</td>
      <td>1.773209</td>
      <td>0.379780</td>
      <td>-0.503198</td>
      <td>1.800499</td>
      <td>0.791461</td>
      <td>0.247676</td>
      <td>-1.514654</td>
      <td>...</td>
      <td>0.247998</td>
      <td>0.771679</td>
      <td>0.909412</td>
      <td>-0.689281</td>
      <td>-0.327642</td>
      <td>-0.139097</td>
      <td>-0.055353</td>
      <td>-0.059752</td>
      <td>378.66</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.0</td>
      <td>-0.966272</td>
      <td>-0.185226</td>
      <td>1.792993</td>
      <td>-0.863291</td>
      <td>-0.010309</td>
      <td>1.247203</td>
      <td>0.237609</td>
      <td>0.377436</td>
      <td>-1.387024</td>
      <td>...</td>
      <td>-0.108300</td>
      <td>0.005274</td>
      <td>-0.190321</td>
      <td>-1.175575</td>
      <td>0.647376</td>
      <td>-0.221929</td>
      <td>0.062723</td>
      <td>0.061458</td>
      <td>123.50</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2.0</td>
      <td>-1.158233</td>
      <td>0.877737</td>
      <td>1.548718</td>
      <td>0.403034</td>
      <td>-0.407193</td>
      <td>0.095921</td>
      <td>0.592941</td>
      <td>-0.270533</td>
      <td>0.817739</td>
      <td>...</td>
      <td>-0.009431</td>
      <td>0.798278</td>
      <td>-0.137458</td>
      <td>0.141267</td>
      <td>-0.206010</td>
      <td>0.502292</td>
      <td>0.219422</td>
      <td>0.215153</td>
      <td>69.99</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 31 columns</p>
</div>

Vamos entender um pouco mais de nosso dataset.

## Análise exploratória dos dados (EDA)

<center>
    <img alt="eda" src="https://raw.githubusercontent.com/chicolucio/credit-card-fraud-detection/master/img/eda.jpg">
</center>

### Resumo estatístico

Vejamos as dimensões:

    Quantidade de entradas: 284807
    Quantidade de variáveis: 31

Como já havíamos discutido na introdução, temos 284807 entradas e 31 variáveis.
Na [descrição da base de dados no
Kaggle](https://www.kaggle.com/mlg-ulb/creditcardfraud), são fornecidas algumas
explicações acerca das variáveis:

- por questões de confidencialidade, não são disponibilizadas as identificações
de boa parte das variáveis originais;
- as representadas como *V1, V2,... V28* são resultado de uma transformação de
[análise de componentes principais -
PCA](https://en.wikipedia.org/wiki/Principal_component_analysis), uma técnica
utilizada para condensar a informação contida em várias variáveis originais em
um conjunto menor de variáveis estatísticas (componentes) com uma perda mínima
de informação. Mais adiante veremos algumas consequências dessa transformação em
nossa análise;
- as variáveis `Time` (tempo) e `Amount` (valor de cada transação) não passaram
por PCA.
    - o tempo se refere ao intervalo, em segundos, entre cada transação e a
    primeira no dataset
- a variável `Class` é a variável alvo:
    - o valor `0` significa transação genuína, legítima
    - o valor `1` significa fraude

Vejamos as informações que os métodos `info` e `describe`, do Pandas, nos
fornecem:

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 284807 entries, 0 to 284806
    Data columns (total 31 columns):
     #   Column  Non-Null Count   Dtype  
    ---  ------  --------------   -----  
     0   Time    284807 non-null  float64
     1   V1      284807 non-null  float64
     2   V2      284807 non-null  float64
     3   V3      284807 non-null  float64
     4   V4      284807 non-null  float64
     5   V5      284807 non-null  float64
     6   V6      284807 non-null  float64
     7   V7      284807 non-null  float64
     8   V8      284807 non-null  float64
     9   V9      284807 non-null  float64
     10  V10     284807 non-null  float64
     11  V11     284807 non-null  float64
     12  V12     284807 non-null  float64
     13  V13     284807 non-null  float64
     14  V14     284807 non-null  float64
     15  V15     284807 non-null  float64
     16  V16     284807 non-null  float64
     17  V17     284807 non-null  float64
     18  V18     284807 non-null  float64
     19  V19     284807 non-null  float64
     20  V20     284807 non-null  float64
     21  V21     284807 non-null  float64
     22  V22     284807 non-null  float64
     23  V23     284807 non-null  float64
     24  V24     284807 non-null  float64
     25  V25     284807 non-null  float64
     26  V26     284807 non-null  float64
     27  V27     284807 non-null  float64
     28  V28     284807 non-null  float64
     29  Amount  284807 non-null  float64
     30  Class   284807 non-null  int64  
    dtypes: float64(30), int64(1)
    memory usage: 67.4 MB


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Time</th>
      <th>V1</th>
      <th>V2</th>
      <th>V3</th>
      <th>V4</th>
      <th>V5</th>
      <th>V6</th>
      <th>V7</th>
      <th>V8</th>
      <th>V9</th>
      <th>V10</th>
      <th>V11</th>
      <th>V12</th>
      <th>V13</th>
      <th>V14</th>
      <th>V15</th>
      <th>V16</th>
      <th>V17</th>
      <th>V18</th>
      <th>V19</th>
      <th>V20</th>
      <th>V21</th>
      <th>V22</th>
      <th>V23</th>
      <th>V24</th>
      <th>V25</th>
      <th>V26</th>
      <th>V27</th>
      <th>V28</th>
      <th>Amount</th>
      <th>Class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>284807.00</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>2.85e+05</td>
      <td>284807.00</td>
      <td>2.85e+05</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>94813.86</td>
      <td>3.92e-15</td>
      <td>5.68e-16</td>
      <td>-8.76e-15</td>
      <td>2.81e-15</td>
      <td>-1.55e-15</td>
      <td>2.04e-15</td>
      <td>-1.70e-15</td>
      <td>-1.89e-16</td>
      <td>-3.15e-15</td>
      <td>1.77e-15</td>
      <td>9.29e-16</td>
      <td>-1.80e-15</td>
      <td>1.67e-15</td>
      <td>1.48e-15</td>
      <td>3.50e-15</td>
      <td>1.39e-15</td>
      <td>-7.47e-16</td>
      <td>4.26e-16</td>
      <td>9.02e-16</td>
      <td>5.13e-16</td>
      <td>1.47e-16</td>
      <td>8.04e-16</td>
      <td>5.28e-16</td>
      <td>4.46e-15</td>
      <td>1.43e-15</td>
      <td>1.70e-15</td>
      <td>-3.66e-16</td>
      <td>-1.22e-16</td>
      <td>88.35</td>
      <td>1.73e-03</td>
    </tr>
    <tr>
      <th>std</th>
      <td>47488.15</td>
      <td>1.96e+00</td>
      <td>1.65e+00</td>
      <td>1.52e+00</td>
      <td>1.42e+00</td>
      <td>1.38e+00</td>
      <td>1.33e+00</td>
      <td>1.24e+00</td>
      <td>1.19e+00</td>
      <td>1.10e+00</td>
      <td>1.09e+00</td>
      <td>1.02e+00</td>
      <td>9.99e-01</td>
      <td>9.95e-01</td>
      <td>9.59e-01</td>
      <td>9.15e-01</td>
      <td>8.76e-01</td>
      <td>8.49e-01</td>
      <td>8.38e-01</td>
      <td>8.14e-01</td>
      <td>7.71e-01</td>
      <td>7.35e-01</td>
      <td>7.26e-01</td>
      <td>6.24e-01</td>
      <td>6.06e-01</td>
      <td>5.21e-01</td>
      <td>4.82e-01</td>
      <td>4.04e-01</td>
      <td>3.30e-01</td>
      <td>250.12</td>
      <td>4.15e-02</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.00</td>
      <td>-5.64e+01</td>
      <td>-7.27e+01</td>
      <td>-4.83e+01</td>
      <td>-5.68e+00</td>
      <td>-1.14e+02</td>
      <td>-2.62e+01</td>
      <td>-4.36e+01</td>
      <td>-7.32e+01</td>
      <td>-1.34e+01</td>
      <td>-2.46e+01</td>
      <td>-4.80e+00</td>
      <td>-1.87e+01</td>
      <td>-5.79e+00</td>
      <td>-1.92e+01</td>
      <td>-4.50e+00</td>
      <td>-1.41e+01</td>
      <td>-2.52e+01</td>
      <td>-9.50e+00</td>
      <td>-7.21e+00</td>
      <td>-5.45e+01</td>
      <td>-3.48e+01</td>
      <td>-1.09e+01</td>
      <td>-4.48e+01</td>
      <td>-2.84e+00</td>
      <td>-1.03e+01</td>
      <td>-2.60e+00</td>
      <td>-2.26e+01</td>
      <td>-1.54e+01</td>
      <td>0.00</td>
      <td>0.00e+00</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>54201.50</td>
      <td>-9.20e-01</td>
      <td>-5.99e-01</td>
      <td>-8.90e-01</td>
      <td>-8.49e-01</td>
      <td>-6.92e-01</td>
      <td>-7.68e-01</td>
      <td>-5.54e-01</td>
      <td>-2.09e-01</td>
      <td>-6.43e-01</td>
      <td>-5.35e-01</td>
      <td>-7.62e-01</td>
      <td>-4.06e-01</td>
      <td>-6.49e-01</td>
      <td>-4.26e-01</td>
      <td>-5.83e-01</td>
      <td>-4.68e-01</td>
      <td>-4.84e-01</td>
      <td>-4.99e-01</td>
      <td>-4.56e-01</td>
      <td>-2.12e-01</td>
      <td>-2.28e-01</td>
      <td>-5.42e-01</td>
      <td>-1.62e-01</td>
      <td>-3.55e-01</td>
      <td>-3.17e-01</td>
      <td>-3.27e-01</td>
      <td>-7.08e-02</td>
      <td>-5.30e-02</td>
      <td>5.60</td>
      <td>0.00e+00</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>84692.00</td>
      <td>1.81e-02</td>
      <td>6.55e-02</td>
      <td>1.80e-01</td>
      <td>-1.98e-02</td>
      <td>-5.43e-02</td>
      <td>-2.74e-01</td>
      <td>4.01e-02</td>
      <td>2.24e-02</td>
      <td>-5.14e-02</td>
      <td>-9.29e-02</td>
      <td>-3.28e-02</td>
      <td>1.40e-01</td>
      <td>-1.36e-02</td>
      <td>5.06e-02</td>
      <td>4.81e-02</td>
      <td>6.64e-02</td>
      <td>-6.57e-02</td>
      <td>-3.64e-03</td>
      <td>3.73e-03</td>
      <td>-6.25e-02</td>
      <td>-2.95e-02</td>
      <td>6.78e-03</td>
      <td>-1.12e-02</td>
      <td>4.10e-02</td>
      <td>1.66e-02</td>
      <td>-5.21e-02</td>
      <td>1.34e-03</td>
      <td>1.12e-02</td>
      <td>22.00</td>
      <td>0.00e+00</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>139320.50</td>
      <td>1.32e+00</td>
      <td>8.04e-01</td>
      <td>1.03e+00</td>
      <td>7.43e-01</td>
      <td>6.12e-01</td>
      <td>3.99e-01</td>
      <td>5.70e-01</td>
      <td>3.27e-01</td>
      <td>5.97e-01</td>
      <td>4.54e-01</td>
      <td>7.40e-01</td>
      <td>6.18e-01</td>
      <td>6.63e-01</td>
      <td>4.93e-01</td>
      <td>6.49e-01</td>
      <td>5.23e-01</td>
      <td>4.00e-01</td>
      <td>5.01e-01</td>
      <td>4.59e-01</td>
      <td>1.33e-01</td>
      <td>1.86e-01</td>
      <td>5.29e-01</td>
      <td>1.48e-01</td>
      <td>4.40e-01</td>
      <td>3.51e-01</td>
      <td>2.41e-01</td>
      <td>9.10e-02</td>
      <td>7.83e-02</td>
      <td>77.16</td>
      <td>0.00e+00</td>
    </tr>
    <tr>
      <th>max</th>
      <td>172792.00</td>
      <td>2.45e+00</td>
      <td>2.21e+01</td>
      <td>9.38e+00</td>
      <td>1.69e+01</td>
      <td>3.48e+01</td>
      <td>7.33e+01</td>
      <td>1.21e+02</td>
      <td>2.00e+01</td>
      <td>1.56e+01</td>
      <td>2.37e+01</td>
      <td>1.20e+01</td>
      <td>7.85e+00</td>
      <td>7.13e+00</td>
      <td>1.05e+01</td>
      <td>8.88e+00</td>
      <td>1.73e+01</td>
      <td>9.25e+00</td>
      <td>5.04e+00</td>
      <td>5.59e+00</td>
      <td>3.94e+01</td>
      <td>2.72e+01</td>
      <td>1.05e+01</td>
      <td>2.25e+01</td>
      <td>4.58e+00</td>
      <td>7.52e+00</td>
      <td>3.52e+00</td>
      <td>3.16e+01</td>
      <td>3.38e+01</td>
      <td>25691.16</td>
      <td>1.00e+00</td>
    </tr>
  </tbody>
</table>
</div>


Vemos que:

- todos as variáveis são de tipos numéricos do tipo *float*, com exceção da
`Class`, nosso alvo, que é do tipo inteiro, possuindo apenas valores 0 ou 1; 
- as variáveis `Vx`, ou seja, *V1, V2,...V28*, possuem faixas de valores
distintas
- a variável `Amount` tem grande faixa de valores: de 0 até 25691.16. No
entanto, predominam transações de pequeno valor já que o intervalo interquartil
(IQR, de 25 a 75 %) vai de 5.60 até 77.16, com mediana de 22.00. A média é de
88.35, acima do limite superior do IQR. Há, portanto, a expectativa de muitos
outliers na faixa de valores altos.

Já vimos com `info` que aparentemente não há dados ausentes no dataset. 

Quando lidamos com dados numéricos, também é uma boa prática verificar se há uma
quantidade não usual de valores zero. Isto porque, de forma errônea, muitos
colocam o valor zero como indicador de valor ausente e tal prática pode levar a
distorções de análise:

    Time           2
    V1             0
    V2             0
    V3             0
    V4             0
    V5             0
    V6             0
    V7             0
    V8             0
    V9             0
    V10            0
    V11            0
    V12            0
    V13            0
    V14            0
    V15            0
    V16            0
    V17            0
    V18            0
    V19            0
    V20            0
    V21            0
    V22            0
    V23            0
    V24            0
    V25            0
    V26            0
    V27            0
    V28            0
    Amount      1825
    Class     284315
    dtype: int64

Por óbvio, `Class` possui muitos valores 0 já que tal valor nessa variável tem o
significado de transação genuína. Vemos que há 1825 entradas de transações de
valor 0 (variável `Amount`). Não está claro o que isto significa, se são casos
de estorno, por exemplo, ou algo similar. Como é um número muito pequeno de
casos frente à quantidade de entradas do dataset, tais entradas não foram
retiradas.

Já vimos na introdução que o dataset é desbalanceado. Verifiquemos tal
informação visualmente:
    
![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_29_0.png)
    

### Visualizando a distribuição de cada variável

Comecemos com histogramas para as variáveis do dataset:

![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_34_0.png)
    

Vemos que todas as features `Vx` possuem valores centrados em zero, uma
característica derivada do processo de PCA que as originou. 

A variável `Time` possui um perfil bimodal. Conforme citado anteriormente, o
dataset é para transações de dois dias seguidos, sendo 0 o tempo inicial e os
demais tempos registrados como o tempo, em segundos, entre cada transação após o
momento inicial. Vemos que a primeira "onda" do perfil termina ao redor de
90.000 segundos, aproximadamente 25 h. Assim, considerando que o tempo zero seja
meia-noite, o perfil bimodal é coerente com um maior registro de transações em
horário comercial em cada dia.

Quanto a `Amount`, vemos uma única barra em valores pequenos. Coerente com o já
discutido de que há grande concentração em valores abaixo de 100.00. 

Em `Class`, o já discutido predomínio do valor 0 referente a transações
legítimas.

Vamos avaliar agora os *boxplots* de cada variável:
    
![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_37_0.png)
    

Novamente, vemos que as features `Vx` estão centradas em zero mas, agora, é
possível ver que possuem faixas de valores bem distintas e diversos outliers.
Quanto a `Amount`, mais uma indicação que possui outliers em valores elevados.

As variáveis `Vx` são resultado de PCA e não temos informações sobre as
variáveis originais. Assim, vamos avaliar como é a distribuição de cada classe
alvo para cada variável. O intuito é tentar identificar quais dessas variáveis
podem ser interessantes se em algum momento do estudo desejarmos selecionar
features para melhorar nossos modelos. Usaremos [gráficos
KDE](https://seaborn.pydata.org/tutorial/distributions.html#tutorial-kde):


![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_39_0.png)
    

Vemos que há variáveis onde a distinção das duas classes é bem clara: *V1-4*,
*V9-12, V14*, e *V16-19*. Logo, podem ser de interesse em modelos. Mais adiante
analisaremos diagramas de correlação que podem ratificar a escolha de tais
variáveis.

Vejamos como é o comportamento de cada classe para a variável `Amount`:

    
![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_42_0.png)
    

Vemos que a mediana dos valores fraudulentos é menor que a de transações
genuínas, embora a média seja maior. Vejamos um breve resumo estatístico das
transações fraudulentas:

    count     492.000000
    mean      122.211321
    std       256.683288
    min         0.000000
    25%         1.000000
    50%         9.250000
    75%       105.890000
    max      2125.870000
    Name: Amount, dtype: float64

Vemos que o valor máximo é de 2125.87, a média de 122.21, e a mediana de 9.25.

### Matriz de correlação

Vamos verificar a matriz de correlação entre as features de nosso dataset:
    
![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_47_0.png)
    

Veja que é uma matrix que não faz muito sentido. Basicamente, informa que não há
correlação alguma (nem positiva nem negativa) entre todas as variáveis `Vx`. Na
realidade, isto é consequência do desbalanceamento da base de dados. Como quase
todos os registros são de classe 0, transações legítimas, não são detectadas
correlações.

Nosso próximo passo será entender o preparo dos dados para lidar com esse
desbalanceamento.

## Preparando os dados

<center>
    <img alt="data" src="https://raw.githubusercontent.com/chicolucio/credit-card-fraud-detection/master/img/data.jpg">
</center>

### Escalando os dados

Como já discutido, as features `Amount` e `Time` não passaram por nenhum tipo de
tratamento antes da disponibilização dos dados. Para alguns algoritmos como, por
exemplo, o K-Nearest Neighbor (KNN), é interessante que os atributos tenham
escalas similares. Há [diversos métodos para escalar os
atributos](https://scikit-learn.org/stable/auto_examples/preprocessing/plot_all_scaling.html).
Aqui, foi utilizado o `RobustScaler` por ser menos sensível a outliers muito
discrepantes como os que ocorrem em `Amount`. Após seu uso, as cinco primeiras
entradas do dataset ficaram assim:

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>V1</th>
      <th>V2</th>
      <th>V3</th>
      <th>V4</th>
      <th>V5</th>
      <th>V6</th>
      <th>V7</th>
      <th>V8</th>
      <th>V9</th>
      <th>V10</th>
      <th>V11</th>
      <th>V12</th>
      <th>V13</th>
      <th>V14</th>
      <th>V15</th>
      <th>V16</th>
      <th>V17</th>
      <th>V18</th>
      <th>V19</th>
      <th>V20</th>
      <th>V21</th>
      <th>V22</th>
      <th>V23</th>
      <th>V24</th>
      <th>V25</th>
      <th>V26</th>
      <th>V27</th>
      <th>V28</th>
      <th>Class</th>
      <th>scaled_amount</th>
      <th>scaled_time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-1.36</td>
      <td>-0.07</td>
      <td>2.54</td>
      <td>1.38</td>
      <td>-0.34</td>
      <td>0.46</td>
      <td>0.24</td>
      <td>0.10</td>
      <td>0.36</td>
      <td>0.09</td>
      <td>-0.55</td>
      <td>-0.62</td>
      <td>-0.99</td>
      <td>-0.31</td>
      <td>1.47</td>
      <td>-0.47</td>
      <td>0.21</td>
      <td>0.03</td>
      <td>0.40</td>
      <td>0.25</td>
      <td>-1.83e-02</td>
      <td>2.78e-01</td>
      <td>-0.11</td>
      <td>0.07</td>
      <td>0.13</td>
      <td>-0.19</td>
      <td>1.34e-01</td>
      <td>-0.02</td>
      <td>0</td>
      <td>1.78</td>
      <td>-0.99</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.19</td>
      <td>0.27</td>
      <td>0.17</td>
      <td>0.45</td>
      <td>0.06</td>
      <td>-0.08</td>
      <td>-0.08</td>
      <td>0.09</td>
      <td>-0.26</td>
      <td>-0.17</td>
      <td>1.61</td>
      <td>1.07</td>
      <td>0.49</td>
      <td>-0.14</td>
      <td>0.64</td>
      <td>0.46</td>
      <td>-0.11</td>
      <td>-0.18</td>
      <td>-0.15</td>
      <td>-0.07</td>
      <td>-2.26e-01</td>
      <td>-6.39e-01</td>
      <td>0.10</td>
      <td>-0.34</td>
      <td>0.17</td>
      <td>0.13</td>
      <td>-8.98e-03</td>
      <td>0.01</td>
      <td>0</td>
      <td>-0.27</td>
      <td>-0.99</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-1.36</td>
      <td>-1.34</td>
      <td>1.77</td>
      <td>0.38</td>
      <td>-0.50</td>
      <td>1.80</td>
      <td>0.79</td>
      <td>0.25</td>
      <td>-1.51</td>
      <td>0.21</td>
      <td>0.62</td>
      <td>0.07</td>
      <td>0.72</td>
      <td>-0.17</td>
      <td>2.35</td>
      <td>-2.89</td>
      <td>1.11</td>
      <td>-0.12</td>
      <td>-2.26</td>
      <td>0.52</td>
      <td>2.48e-01</td>
      <td>7.72e-01</td>
      <td>0.91</td>
      <td>-0.69</td>
      <td>-0.33</td>
      <td>-0.14</td>
      <td>-5.54e-02</td>
      <td>-0.06</td>
      <td>0</td>
      <td>4.98</td>
      <td>-0.99</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.97</td>
      <td>-0.19</td>
      <td>1.79</td>
      <td>-0.86</td>
      <td>-0.01</td>
      <td>1.25</td>
      <td>0.24</td>
      <td>0.38</td>
      <td>-1.39</td>
      <td>-0.05</td>
      <td>-0.23</td>
      <td>0.18</td>
      <td>0.51</td>
      <td>-0.29</td>
      <td>-0.63</td>
      <td>-1.06</td>
      <td>-0.68</td>
      <td>1.97</td>
      <td>-1.23</td>
      <td>-0.21</td>
      <td>-1.08e-01</td>
      <td>5.27e-03</td>
      <td>-0.19</td>
      <td>-1.18</td>
      <td>0.65</td>
      <td>-0.22</td>
      <td>6.27e-02</td>
      <td>0.06</td>
      <td>0</td>
      <td>1.42</td>
      <td>-0.99</td>
    </tr>
    <tr>
      <th>4</th>
      <td>-1.16</td>
      <td>0.88</td>
      <td>1.55</td>
      <td>0.40</td>
      <td>-0.41</td>
      <td>0.10</td>
      <td>0.59</td>
      <td>-0.27</td>
      <td>0.82</td>
      <td>0.75</td>
      <td>-0.82</td>
      <td>0.54</td>
      <td>1.35</td>
      <td>-1.12</td>
      <td>0.18</td>
      <td>-0.45</td>
      <td>-0.24</td>
      <td>-0.04</td>
      <td>0.80</td>
      <td>0.41</td>
      <td>-9.43e-03</td>
      <td>7.98e-01</td>
      <td>-0.14</td>
      <td>0.14</td>
      <td>-0.21</td>
      <td>0.50</td>
      <td>2.19e-01</td>
      <td>0.22</td>
      <td>0</td>
      <td>0.67</td>
      <td>-0.99</td>
    </tr>
  </tbody>
</table>
</div>


### Dividindo o dataset (treino e teste)

Para facilitar a reprodutibilidade deste estudo, em métodos que possuem
parâmetros para controlar a semente de geração randômica, a semente
foi fixada conforme pode ser verificado no [código-fonte do
projeto](https://github.com/chicolucio/credit-card-fraud-detection).
Assim, os resultados a seguir podem ser reproduzidos pelo leitor interessado.

Em bases de dados com grande desbalanço na classe alvo, como o caso, é
importante que a porção da base separada para treino dos modelos tenha a mesma
proporção entre as classes da porção separada para teste. Em um primeiro
momento, foi feita uma simples divisão com o método `train_test_split`
do Scikit-Learn passando ao parâmetro `stratify` a variável alvo para que a
divisão seja feita com a proporção desta. 
Temos, então, pelas configurações padrão do método, 75 % do dataset separado
para treino e os 25 % restantes, para teste:

| Treino | Teste |
| ------ | ----- |
| [213236,    369] | [71079,   123]

Ambas as porções possuem o mesmo desbalanceamento:

| Treino / % | Teste / % |
| ------ | ----- |
| [99.827, 0.173] | [99.827,  0.173]


### Balanceando o dataset

Como vimos na matriz de correlação construída anteriormente, o desbalanceamento
pode ser prejudicial para visualizar variáveis importantes e alguns algoritmos
podem ter sua performance prejudicada com dados desbalanceados. Existem várias
estratégias para lidar com esse problema:

- Reestruturar os dados:
    - Undersampling: reduzir o número de observações da classe majoritária para
    diminuir a diferença entre as categorias
    - Oversampling: criar sinteticamente novas observações da classe
    minoritária, igualando a proporção das categorias
- Utilizar um algoritmo mais resistente a problemas de desbalanceamento
- Se possível, coletar mais dados
- Usar modelos penalizados, ou seja, que penalizem mais erros de classificação
da categoria minoritária
- Selecionar cuidadosamente a métrica de treino, selecionando uma que avalie
mais cuidadosamente erros da classe minoritária

A única estratégia que não tivemos como adotar neste estudo foi a coleta de mais
dados, trata-se de um dataset estático fornecido. Todas as demais estratégias
tiveram participação neste estudo. Comecemos, então, pela reestruturação dos
dados.

Temos um dataset de tamanho significativo de forma que adotar a estratégia de
*undersampling* pode ser interessante. Dentre as técnicas de undersampling, a
utilizada neste estudo foi a retirada aleatória de dados da classe majoritária,
a *Random Undersampling*, através do `RandomUnderSampler` da biblioteca
Imbalanced-Learning. Vejamos o resultado:

    
![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_64_1.png)
    
    0    369
    1    369
    Name: Class, dtype: int64

Cabe destacar que o undersampling é realizado apenas na base de treino, gerando
uma variável alvo que contém iguais quantidades. No caso, 369 para cada classe.
Vamos gerar um dataset com esses dados balanceados e analisá-lo:


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>V1</th>
      <th>V2</th>
      <th>V3</th>
      <th>V4</th>
      <th>V5</th>
      <th>V6</th>
      <th>V7</th>
      <th>V8</th>
      <th>V9</th>
      <th>V10</th>
      <th>...</th>
      <th>V22</th>
      <th>V23</th>
      <th>V24</th>
      <th>V25</th>
      <th>V26</th>
      <th>V27</th>
      <th>V28</th>
      <th>scaled_amount</th>
      <th>scaled_time</th>
      <th>Class</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-0.245994</td>
      <td>0.864456</td>
      <td>0.138314</td>
      <td>-0.627707</td>
      <td>0.440627</td>
      <td>-1.072989</td>
      <td>0.941862</td>
      <td>-0.239466</td>
      <td>0.197495</td>
      <td>-0.168526</td>
      <td>...</td>
      <td>-0.640062</td>
      <td>0.139061</td>
      <td>-0.135690</td>
      <td>-0.494629</td>
      <td>0.127075</td>
      <td>-0.017464</td>
      <td>0.087222</td>
      <td>-0.269825</td>
      <td>0.765246</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.169390</td>
      <td>-0.874467</td>
      <td>-1.661547</td>
      <td>-0.993435</td>
      <td>-0.218378</td>
      <td>0.253062</td>
      <td>-1.196605</td>
      <td>0.260048</td>
      <td>0.130298</td>
      <td>0.290238</td>
      <td>...</td>
      <td>0.530401</td>
      <td>0.028349</td>
      <td>-0.505492</td>
      <td>-0.098210</td>
      <td>-0.066314</td>
      <td>0.010599</td>
      <td>-0.029377</td>
      <td>-0.055893</td>
      <td>0.787028</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>-1.466046</td>
      <td>0.946201</td>
      <td>-1.508302</td>
      <td>-0.918113</td>
      <td>2.266431</td>
      <td>0.770354</td>
      <td>0.767603</td>
      <td>0.618772</td>
      <td>-0.806902</td>
      <td>-0.968734</td>
      <td>...</td>
      <td>0.534899</td>
      <td>-0.426521</td>
      <td>-0.988301</td>
      <td>0.270773</td>
      <td>0.762529</td>
      <td>-0.492720</td>
      <td>0.112220</td>
      <td>-0.167819</td>
      <td>0.597657</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>-0.777055</td>
      <td>-0.117475</td>
      <td>0.723841</td>
      <td>-3.492441</td>
      <td>-0.052029</td>
      <td>0.571674</td>
      <td>-0.439675</td>
      <td>-0.503063</td>
      <td>-2.027951</td>
      <td>0.902438</td>
      <td>...</td>
      <td>-0.705686</td>
      <td>-0.395879</td>
      <td>-0.442375</td>
      <td>0.853427</td>
      <td>-0.076788</td>
      <td>0.281768</td>
      <td>0.090274</td>
      <td>-0.167680</td>
      <td>0.592183</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1.308457</td>
      <td>-1.176200</td>
      <td>-0.022869</td>
      <td>-1.397037</td>
      <td>-1.222895</td>
      <td>-0.639747</td>
      <td>-0.643778</td>
      <td>-0.054389</td>
      <td>-2.277189</td>
      <td>1.584824</td>
      <td>...</td>
      <td>-0.995555</td>
      <td>0.148532</td>
      <td>0.143905</td>
      <td>0.140944</td>
      <td>-0.453468</td>
      <td>-0.005713</td>
      <td>0.013661</td>
      <td>0.878921</td>
      <td>-0.359649</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 31 columns</p>
</div>


    Quantidade de entradas: 738
    Quantidade de variáveis: 31


#### Matriz de correlação e principais features

Vejamos como é a matriz de correlação deste novo dataset:

    
![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_70_0.png)
    

Agora temos uma matriz de correlação usual. Vemos que há várias features que se
correlacionam entre si, positiva ou negativamente, e conseguimos identificar
como cada feature se correlaciona com nossa variável alvo. Vamos separar as
features entre as que se correlacionam positivamente e as que se correlacionam
negativamente com a variável alvo.


Correlação negativa:

    V14           -0.749573
    V12           -0.682799
    V10           -0.632799
    V16           -0.587845
    V9            -0.571227
    V3            -0.569337
    V17           -0.561425
    V7            -0.478392
    V18           -0.466059
    V1            -0.436271
    V6            -0.407284
    V5            -0.390976
    scaled_time   -0.148974
    V15           -0.090455
    V24           -0.081980
    V13           -0.069750
    V23           -0.027070
    Name: Class, dtype: float64

Correlação positiva:

    Class            1.000000
    V4               0.719943
    V11              0.685746
    V2               0.503553
    V19              0.276873
    V20              0.132063
    V21              0.129718
    scaled_amount    0.075594
    V27              0.073456
    V28              0.071199
    V8               0.060089
    V26              0.038927
    V22              0.035328
    V25              0.008493
    Name: Class, dtype: float64


Apenas para visualização e melhor compreensão, selecionemos as 4 features com
maior correlação negativa e as 4 com maior correlação positiva:

- Negativa: V10, V12, V14, V16
- Positiva: V2, V4, V11, V19

Vejamos os diagramas *boxplot* dessas features separando as classes de
transações:

    
![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_76_0.png)
    

![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_77_0.png)
    

Observe que, nas features de correlação negativa com a feature alvo, temos que a
faixa de valores das transações fraudulentas é menor que a de transações
genuínas. O contrário é observado para as features com correlação positiva.

Assim, caso no decorrer do estudo se resolva focar em algumas features já
sabemos quais parecem as mais relevantes. Esse conhecimento pode ser útil, por
exemplo, para fazer uma limpeza de outliers nessas features e verificar se há
melhora nos modelos.

Como o *random undersampling* retira amostras aleatórias do dataset original,
vejamos se o mesmo perfil de comportamento se observa para estas features no
dataset original:

    
![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_79_0.png)
    


![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_80_0.png)
    

Observa-se o mesmo comportamento.

Comecemos, então, a verificar como foi o treinamento dos modelos.

## Avaliando modelos

<center>
    <img alt="ai" src="https://raw.githubusercontent.com/chicolucio/credit-card-fraud-detection/master/img/ai.jpg">
</center>

### Um pouco sobre métricas

Quando falamos de modelos, sempre surge a discussão de qual ou quais métricas
serão consideradas para avaliar a performance comparativa de modelos para um
determinado contexto. A [documentação do
Scikit-Learn](https://scikit-learn.org/stable/modules/model_evaluation.html)
possui muitas informações sobre o assunto e a Wikipedia possui [um artigo apenas
para métricas de classificações
binárias](https://en.wikipedia.org/wiki/Evaluation_of_binary_classifiers). Aqui,
vamos abordar o mínimo necessário para entender a escolha de métrica feita no
estudo.

#### Matriz de confusão

Comecemos lembrando de uma matriz de confusão binária e de algumas terminologias
importantes:

<center>
    <img alt="cm" src="https://raw.githubusercontent.com/chicolucio/credit-card-fraud-detection/master/img/confusion_matrix.png">
</center>

#### Acurácia

A métrica mais famosa, e, talvez, a mais erroneamente utilizada, é a
**acurácia**. Para uma classificação binária:

$$
\text{acurácia} = \frac{TP + TN}{TP + TN + FP + FN}
$$

Veja, pela definição acima, que é uma métrica que considera a fração de acertos
no total de predições. Logo, se a base de dados for muito desbalanceada, como no
nosso caso onde mais de 99 % das entradas são de uma das classes, altas acurácias
serão obtidas facilmente. Afinal, basta prever que todas as entradas são da
classe dominante e já teremos acurácias da ordem de 99 %. Caso tenha ficado com
dúvidas, veja [este artigo sobre o paradoxo da
acurácia](https://en.wikipedia.org/wiki/Accuracy_paradox), que é o nome deste
fenômeno. Veremos isto na prática mais adiante.

#### Precisão e recall

Em situações desbalanceadas, duas outras métricas costumam ser usadas no lugar
da acurácia, a **precisão** e o **recall**, com as seguintes definições:

$$
\text{precisão} = \frac{TP}{TP + FP} \qquad \text{recall} = \frac{TP}{TP + FN}
$$

Vemos que a precisão é a fração de verdadeiros positivos frente o total de
previsões positivas. Ou seja, quanto seu modelo é bom em prever positivos.
Buscar maximizar a precisão irá minimizar o número de erros de falsos positivos.

Já o recall é fração de verdadeiros positivos frente ao total de positivos
reais. Afinal, os falsos negativos são, na realidade, positivos. Logo, o recall
quantifica a proporção de reais positivos que foram identificados. Buscar
maximizar o recall irá minimizar o número de erros de falsos negativos.

Considere nosso contexto de fraude em cartões de crédito. Ao classificar
erroneamente uma transação fraudulenta como legítima, um usuário teve seu
dinheiro roubado via, por exemplo, clonagem do cartão. A companhia do cartão
terá que reembolsar o usuário. Por outro lado, uma transação legítima ser
considerada fraudulenta significa que o usuário terá seu cartão bloqueado
indevidamente, causando conflito e desconforto no cliente.

#### F-measure

Observe que há um *trade-off* entre essas duas métricas. Uma busca por aumentar
a precisão pode diminuir o recall e vice-versa. Assim, há uma métrica, a
**F-measure** (medida F), que é a média harmônica entre precisão e recall:

$$
\text{F-measure} = \frac{2 \times \text{precisão} \times \text{recall}}{\text{precisão} + \text{recall}}
$$

#### AUROC

Podemos avaliar este trade-off por outras métricas. O recall também é chamado de
*TPR - true positive rate* (taxa de verdadeiros positivos). Há, também, a
chamada *FPR - false positive rate* (taxa de falsos positivos) definida por:

$$
\text{FPR} = \frac{FP}{TN + FP}
$$

Com estas duas definições, podemos construir um gráfico de **Receiver Operating
Characteristics (ROC)** ou, em português, Curva Característica de Operação do
Receptor. Tal curva, para um dado modelo de classificação binária, mostra o
trade-off de tal modelo entre benefícios (verdadeiros positivos) e custos
(falsos positivos):

<center>    
<img alt='ROC curves' src='https://upload.wikimedia.org/wikipedia/commons/thumb/3/3d/Curva_ROC.svg/640px-Curva_ROC.svg.png'>
</center>

<center>    
    <p style="font-size:80%;color:gray">Curva ROC. <a href="https://pt.wikipedia.org/wiki/Caracter%C3%ADstica_de_Opera%C3%A7%C3%A3o_do_Receptor">Fonte.</a></p>
</center>

A métrica utilizada para comparar diferentes modelos é a área sob a curva 
(**AUC - area under the curve**),  daí ser comum a sigla AUROC.

<center>    
<img alt='AUC' src='https://paulvanderlaken.files.wordpress.com/2019/08/95971-1pk05qgzowhcgriifbz-okq.png'>
</center>

<center>    
    <p style="font-size:80%;color:gray">Área sob a curva (AUC). <a href="https://towardsdatascience.com/understanding-auc-roc-curve-68b2303cc9c5">Fonte.</a></p>
</center>

Na animação abaixo, vemos o comportamento de um modelo. Veja como a busca por
mais verdadeiros positivos aumenta a taxa de falsos positivos e como a busca por
diminuir falsos positivos leva à diminuição de verdadeiros positivos.

<img alt='AUC cutoff' src='https://raw.githubusercontent.com/dariyasydykova/open_projects/master/ROC_animation/animations/cutoff.gif'>

<center>    
    <p style="font-size:80%;color:gray">AUROC e distribuições das classes. <a href="https://github.com/dariyasydykova/open_projects">Fonte.</a></p>
</center>

O formato da curva ROC muda de acordo com a capacidade que o  modelo tem de
classificar corretamente cada classe. A animação a seguir começa com as
distribuições de cada classe sobrepostas, ou seja, o modelo não consegue
distinguir as classes. Nessa situação AUC = 0.5, equivalente à um classificador
aleatório ("um chute"). À medida que o modelo consegue separar mais as classes,
AUC aumenta até a classificação perfeita, AUC = 1, onde a curva forma um
ângulo reto.

<img alt='AUC sep' src='https://raw.githubusercontent.com/dariyasydykova/open_projects/master/ROC_animation/animations/ROC.gif'>

<center>    
    <p style="font-size:80%;color:gray">AUROC e capacidade de distinção de classes do modelo. <a href="https://github.com/dariyasydykova/open_projects">Fonte.</a></p>
</center>

#### AUPRC

A AUROC é uma métrica bastante utilizada, especialmente com a finalidade já
citada de selecionar modelos que maximizem a área. No entanto, [trabalhos na
literatura](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0118432)
demonstram que há uma métrica melhor para datasets desbalanceados como o que
estamos estudando aqui.

Já vimos as definições de precisão e de recall e como há um trade-off na busca
por melhorar um ou outro. Isto também pode ser representado graficamente gerando
uma curva e calculando a área sob esta curva. É a **AUPRC** 
(*area under precision-recall curve*). Vejamos como a AUROC e a AUPRC se
comportam com a mudança na capacidade de um modelo de identificar bem duas
classes na animação a seguir:

<img alt='AUC PR' src='https://raw.githubusercontent.com/dariyasydykova/open_projects/master/ROC_animation/animations/PR.gif'>

<center>    
    <p style="font-size:80%;color:gray">AUROC vs AUPRC e capacidade de distinção de classes do modelo. <a href="https://github.com/dariyasydykova/open_projects">Fonte.</a></p>
</center>

Veja que as duas curvas respondem significativamente a mudanças na capacidade de
classificação.

Agora, observe as animações a seguir:

<img alt='AUPRC imb1' src='https://raw.githubusercontent.com/dariyasydykova/open_projects/master/ROC_animation/animations/imbalance.gif'>

<img alt='AUPRC imb2' src='https://raw.githubusercontent.com/dariyasydykova/open_projects/master/ROC_animation/animations/imbalance2.gif'>

<center>    
    <p style="font-size:80%;color:gray">AUROC vs AUPRC em bases de dados desbalanceadas. <a href="https://github.com/dariyasydykova/open_projects">Fonte.</a></p>
</center>

Vemos nas duas animações o efeito de um grande desbalanceamento entre as
classes. Veja que a AUROC permanece quase inalterada, pois o formato da curva
ROC quase não se altera. Já o formato da PRC muda bastante e, consequentemente,
a AUPRC. Não por acaso, a [página do dataset no
Kaggle](https://www.kaggle.com/mlg-ulb/creditcardfraud) recomenda o uso da
AUPRC.

### Nossos objetivos

Portanto, tendo em vista o discutido anteriormente:

> Usamos a AUPRC para selecionar os modelos mais promissores

> Após a seleção, foi realizado um estudo de otimização de hiperparâmetros para
tais modelos

> Em tal otimização, avaliamos o resultado de uma busca por maiores AUPRC e
fizemos a comparação com uma busca por maiores valores de recall

### Definindo uma referência de performance

Foi criado um *baseline*, uma referência, para nossa avaliação. Assim, sabemos
que modelos que têm desempenho menor que nossa referência podem ser descartados.

Para criar nossa referência, usamos a classe `DummyClassifier` do Scikit-Learn
com a estratégia `constant`, recomendada como referência para classificadores
que buscam ter boa performance em classes minoritárias [segundo a
documentação](https://scikit-learn.org/stable/modules/generated/sklearn.dummy.DummyClassifier.html).
Já vimos, na parte de preparo de dados de nosso estudo, que devemos usar
estratégias estratificadas para que treino e teste tenham a mesma proporção
entre as classes. Usamos, então, o `RepeatedStratifiedKFold` para fazer diversos
splits (divisões) treino/teste e `cross_validate` para retornar o resultado
consolidado de todas os splits por validação cruzada. Vejamos os resultados
de várias métricas para nossa referência:

        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.002    |   0.000   
    precision       |   0.002    |   0.000   
    recall          |   1.000    |   0.000   
    f1_score        |   0.003    |   0.000   
    auprc           |   0.501    |   0.000   
    auroc           |   0.500    |   0.000   


Veja que o `auprc`, que representa o AUPRC visto anteriormente, retornou valor
0.5. Ou seja, se comportou como um classificador aleatório. Escolhemos
classificadores que se comportaram melhor que este baseline.

### Aplicando no dataset completo

Discutimos anteriormente que há potenciais ganhos em adotar estratégias de
balanceamento de dataset quando temos casos onde uma das classes é muito menor
que outra. Antes, treinamos alguns modelos no dataset completo, ou seja,
desbalanceado, para que os resultados servissem de base de comparação para os
resultados que obtemos mais adiante com estratégias de balanceamento.

Para esse estudo, analisamos seis algoritmos de classificação:

- Algoritmos lineares:
    - Regressão logística (*Logistic Regression* - LR)
    - Análise de discriminante linear (*Linear Discriminat Analysis* - LDA)
- Algoritmos não lineares:
    - k vizinhos mais próximos (*k-Nearest Neighbors* - KNN)
    - *Naive Bayes* - NB
    - Árvore de decisão (*Classification and Regression Trees* - CART)
    - Máquina de vetores de suporte (*Support Vector Machines* - SVM)

Para o dataset completo, que possui mais de 280 mil entradas, não utilizamos o
KNN nem o SVM por serem algoritmos que demandam elevada potência computacional
em grandes datasets. Os utilizamos mais adiante, quando adotamos estratégias
de undersampling.

Os resultados obtidos foram:

    LR
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.999    |   0.000   
    precision       |   0.875    |   0.034   
    recall          |   0.624    |   0.044   
    f1_score        |   0.727    |   0.027   
    auprc           |   0.758    |   0.024   
    auroc           |   0.812    |   0.022   
    
    LDA
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.999    |   0.000   
    precision       |   0.866    |   0.026   
    recall          |   0.766    |   0.018   
    f1_score        |   0.812    |   0.013   
    auprc           |   0.782    |   0.025   
    auroc           |   0.883    |   0.009   
    
    CART
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.999    |   0.000   
    precision       |   0.753    |   0.029   
    recall          |   0.759    |   0.030   
    f1_score        |   0.755    |   0.021   
    auprc           |   0.756    |   0.020   
    auroc           |   0.880    |   0.015   
    
    NB
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.978    |   0.001   
    precision       |   0.062    |   0.003   
    recall          |   0.829    |   0.020   
    f1_score        |   0.115    |   0.005   
    auprc           |   0.427    |   0.013   
    auroc           |   0.904    |   0.010   
    

Veja que todos os modelos obtiveram resultados de acurácia muito altos. Isto
corrobora o discutido anteriormente sobre o paradoxo da acurácia, mostrando
porque tal métrica não é adequada com datasets desbalanceados.

Também vemos que os valores de AUROC de todos os modelos são elevados. No
entanto, Naive Bayes, modelo com maior AUROC, possui o menor AUPRC. Inclusive,
menor que nosso baseline. Vemos, aqui, a importância de utilizar métricas que
sejam condizentes com o problema a ser resolvido.

É bastante comum representar os resultados de modelos de classificação binária
na forma de matrizes de confusão. Como estamos fazendo uma estratégia de
repetição, uma função foi utilizada para retornar a média dos resultados das
repetições.  As seguintes matrizes foram obtidas para cada modelo:

    
![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_104_0.png)
    

Veja como o modelo Naive Bayes tem um número muito maior de falsos positivos.

Matrizes de confusão normalizadas permitem visualizar melhor as proporções. 
Nesse tipo de matriz, a soma de cada linha da matriz é 1:

    
![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_106_0.png)
    

Acredito que essa forma de visualizar permite uma melhor comparação entre os
modelos, permitindo compreender a proporção dos resultados. Perceba como o Naive
Bayes foi o único que teve uma fração significativa de falsos positivos, o que
levou ao seu menor valor de AUPRC comparado aos demais modelos.

### Aplicando undersampling

Vimos anteriormente que a estratégia de undersampling para balancear o dataset
permitiu descobrir correlações entre variáveis. No entanto, fazer apenas uma
divisão de treino/teste e aplicar o undersampling não é o ideal. O ideal, sempre
que possível, é fazer várias divisões e avaliar o consolidado. Assim, precisamos
aplicar o undersampling em cada divisão realizada. Para isso, usamos uma função
na qual criamos um *pipeline* para utilizar o `RandomUnderSampler` do
Imbalanced-Learn em cada divisão antes de se avaliar os modelos.

Nesta etapa, foram incluídos os modelos KNN e SVM (SVC no Scikit-Learn, onde o
"C" significa *classifier*) tendo em vista que estamos com datasets menores.
Vejamos os resultados:

    LR
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.961    |   0.006   
    precision       |   0.040    |   0.007   
    recall          |   0.912    |   0.015   
    f1_score        |   0.077    |   0.013   
    auprc           |   0.578    |   0.100   
    auroc           |   0.937    |   0.005   
    
    LDA
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.980    |   0.004   
    precision       |   0.070    |   0.014   
    recall          |   0.839    |   0.017   
    f1_score        |   0.129    |   0.024   
    auprc           |   0.171    |   0.047   
    auroc           |   0.910    |   0.007   
    
    KNN
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.972    |   0.007   
    precision       |   0.056    |   0.012   
    recall          |   0.894    |   0.019   
    f1_score        |   0.105    |   0.021   
    auprc           |   0.601    |   0.065   
    auroc           |   0.933    |   0.008   
    
    CART
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.892    |   0.014   
    precision       |   0.014    |   0.002   
    recall          |   0.901    |   0.018   
    f1_score        |   0.028    |   0.003   
    auprc           |   0.458    |   0.008   
    auroc           |   0.897    |   0.007   
    
    NB
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.966    |   0.005   
    precision       |   0.044    |   0.008   
    recall          |   0.863    |   0.007   
    f1_score        |   0.084    |   0.013   
    auprc           |   0.434    |   0.009   
    auroc           |   0.915    |   0.003   
    
    SVC
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.986    |   0.003   
    precision       |   0.101    |   0.019   
    recall          |   0.875    |   0.013   
    f1_score        |   0.180    |   0.031   
    auprc           |   0.697    |   0.022   
    auroc           |   0.931    |   0.006   
    

Seguindo nossos objetivos, foram selecionados os modelos com maior AUPRC:
LR, KNN e SVC. As seguintes matrizes foram obtidas para os três modelos
selecionados:


![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_120_0.png)
    

Veja que os três modelos escolhidos foram mais eficazes em prever os casos
verdadeiros de fraude do que os modelos aplicados ao dataset completo. Ou seja,
resultaram em maiores valores de recall. Em especial, a regressão logística, LR,
conseguiu detectar mais casos verdadeiros de fraude do que o mesmo modelo
aplicado ao dataset completo. No entanto, isto levou a um aumento de falsos
positivos quando comparado ao mesmo modelo no dataset completo. Como já descrito
anteriormente, este trade-off é uma constante neste tipo de análise.

## Otimizando os hiperparâmetros dos modelos escolhidos

<center>
    <img alt="" src="https://raw.githubusercontent.com/chicolucio/credit-card-fraud-detection/master/img/performance.jpg">
</center>

Hiperparâmetros são parâmetros de modelos que devem ser definidos antes de
treinar o modelo. Foi utilizado o `GridSearchCV` do Scikit-Learn para testar
todas as combinações possíveis dos hiperparâmetros passados. 
A otimização foi feita com os três modelos escolhidos anteriormente. 
### Métrica: AUPRC

O `GridSearchCV` retorna os melhores hiperparâmetros de acordo com alguma
métrica. Como estamos discutindo desde o início deste estudo, a AUPRC é uma
métrica que tem se mostrado mais adequada ao nosso contexto. Vejamos os melhores
hiperparâmetros para cada modelo por essa métrica:

    [LogisticRegression(C=0.001, max_iter=700),
     KNeighborsClassifier(),
     SVC(C=0.5, probability=True)]

Vejamos como tal otimização refletiu em nossas métricas:

    LR
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.998    |   0.000   
    precision       |   0.479    |   0.067   
    recall          |   0.825    |   0.004   
    f1_score        |   0.603    |   0.053   
    auprc           |   0.702    |   0.027   
    auroc           |   0.911    |   0.002   
    
    KNN
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.972    |   0.007   
    precision       |   0.056    |   0.012   
    recall          |   0.894    |   0.019   
    f1_score        |   0.105    |   0.021   
    auprc           |   0.601    |   0.065   
    auroc           |   0.933    |   0.008   
    
    SVC
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.989    |   0.002   
    precision       |   0.124    |   0.018   
    recall          |   0.859    |   0.009   
    f1_score        |   0.217    |   0.027   
    auprc           |   0.707    |   0.015   
    auroc           |   0.924    |   0.004   
    

Vemos que, efetivamente, os valores AUPRC para LR e SVC foram maiores do que
antes da otimização. O KNN permanece inalterado pois a otimização resultou nos
hiperparâmetros que já são utilizados por padrão no modelo. Vejamos isto em
matrizes de confusão normalizadas:

    
![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_137_0.png)
    

Conforme estamos discutindo durante todo o estudo, há um trade-off entre
precisão e recall. Assim, para os modelos LR e SVC, o resultado foi um aumento
da precisão e diminuição do recall, com a consequente diminuição dos falsos
positivos e aumento dos falsos negativos.

Mas, e se escolhêssemos maximizar a detecção de casos de fraude?

### Métrica: recall

Muitos podem pensar que uma determinada instituição financeira iria buscar
maximizar a detecção de fraude, ou seja, aumentar o recall minimizando os falsos
negativos. Vejamos as consequências desta estratégia. 

Eis os resultados da otimização de hiperparâmetros para esta métrica:

    [LogisticRegression(C=0.01, max_iter=700, penalty='l1', solver='liblinear'),
     KNeighborsClassifier(n_neighbors=2, weights='distance'),
     SVC(C=0.5, kernel='linear', probability=True)]

Vamos verificar as métricas obtidas:

    LR
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.563    |   0.007   
    precision       |   0.004    |   0.000   
    recall          |   0.967    |   0.012   
    f1_score        |   0.008    |   0.000   
    auprc           |   0.681    |   0.020   
    auroc           |   0.765    |   0.007   
    
    KNN
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.932    |   0.007   
    precision       |   0.023    |   0.002   
    recall          |   0.919    |   0.014   
    f1_score        |   0.045    |   0.004   
    auprc           |   0.482    |   0.010   
    auroc           |   0.926    |   0.004   
    
    SVC
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    accuracy        |   0.963    |   0.008   
    precision       |   0.043    |   0.008   
    recall          |   0.914    |   0.014   
    f1_score        |   0.081    |   0.014   
    auprc           |   0.532    |   0.131   
    auroc           |   0.939    |   0.005   
    

Vemos que, efetivamente, os valores de recall aumentaram às custas de diminuição
de precisão e de AUPRC. Vejamos as matrizes de confusão:


![png](../projeto_fraude_cartao_credito_files/projeto_fraude_cartao_credito_151_0.png)
    

Observe que a regressão logística teve o maior valor de recall, mas o custo foi
ter uma proporção significativa de falsos positivos. Seria este o cenário ideal?
Os demais modelos também tiveram um aumento nos falsos positivos comparado ao
cenário anterior onde maximizamos o AUPRC, mas em menor escala. O SVC possui a
melhor precisão.

### Devemos maximizar o recall?

Ao classificar erroneamente uma transação fraudulenta como legítima, um usuário
teve seu dinheiro roubado via, por exemplo, clonagem do cartão. A companhia do
cartão terá que reembolsar o usuário. Por outro lado, uma transação legítima ser
considerada fraudulenta significa que o usuário terá seu cartão bloqueado
indevidamente, causando conflito, constrangimento e desconforto no cliente.

Os dois cenários possuem custos distintos e o ideal seria fazer uma análise de
valor esperado para cada modelo considerando tais custos. Não temos tais dados
para o dataset em estudo, mas o aprendizado vale para qualquer contexto. Não há
uma métrica que é a resposta para todos os contextos, cada caso deve ser
analisado separadamente.

## Conclusão

<center>
    <img alt="" src="https://raw.githubusercontent.com/chicolucio/credit-card-fraud-detection/master/img/conclusion.jpg">
</center>

Neste estudo pudemos fazer diversas discussões que são recorrentes em datasets
desbalanceados, dentre elas:

- como balancear o dataset?
- como identificar correlação entre as variáveis?
- qual métrica escolher para identificar o melhor modelo?
- como lidar com o trade-off entre precisão e recall?

Os modelos mais promissores foram o KNN e o SVC. Outros estudos podem ser feitos
buscando avaliar:

- outros modelos não utilizados aqui
- limpeza de outliers de features relevantes

Como de costume em problemas de ciência de dados, não há uma única resposta e
sempre há espaço para melhorias. 

Caso tenha dúvidas, comentários e/ou críticas construtivas me procure:

<div align="center">

<a href="https://www.linkedin.com/in/flsbustamante" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 
<br>
<a href="https://franciscobustamante.com.br" target="_blank"><img src="https://img.shields.io/badge/portfolio-000000?style=for-the-badge&logo=About.me&logoColor=white" target="_blank"></a>
<br>
<a href="https://github.com/chicolucio" target="_blank"><img src="https://github.githubassets.com/images/modules/logos_page/GitHub-Logo.png" target="_blank" width="10%"></a>

</div>
