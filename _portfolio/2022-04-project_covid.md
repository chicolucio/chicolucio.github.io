---
name: Global overview of COVID-19 - The omicron spread
title: Panorama do COVID-19 no mundo
image: /portfolio/projeto_covid_files_april_2022/covid_logo.jpg
position: 1
period: May 2022
toc: true
header:
  image: /portfolio/projeto_covid_files_april_2022/covid_banner.jpg
  og_image: /portfolio/projeto_covid_files_april_2022/covid_logo.jpg
description: |
  It has been 2 years since the emergence of the COVID-19 virus.
  The pandemic control measures adopted by governments around the world have 
  significantly affected everyone's routine. Now, most governments are
  relaxing COVID restrictions despite high omicron variant spread. What can
  we learn from COVID data?
---

## Abstract

It has been 2 years since the emergence of the COVID-19 virus.
The pandemic control measures adopted by governments around the world have 
significantly affected everyone's routine. Now, most governments are
relaxing COVID restrictions despite high omicron variant spread. 
In this study, Our World in Data (OWID) database was used to see the evolution of
cases, deaths, and vaccination. The impact of new variants on cases and deaths
was shown as well as a possible relation with winter season. Some countries were
selected to a more detailed view in order to compare the ones that did not adopt
restrict measures with those that did.

The following page is written in Brazilian Portuguese.

## Introdução

A COVID-19 é uma doença infecciosa causada por um recém-descoberto coronavírus.

Completamos pouco mais de 2 anos desde o surgimento do vírus COVID-19. As medidas para controle pandêmico adotadas por governos em todo mundo  afetaram significativamente a rotina de todos. É de grande interesse acompanharmos a evolução da pandemia para saber seus reais efeitos e avaliar o retorno ao convívio normal.

<center><img alt="covid_banner" width="70%" src="https://raw.githubusercontent.com/chicolucio/panorama-covid-mundo/master/images/evolucao_casos_banner.gif"></center>

Neste trabalho, uma análise exploratória dos dados fornecidos pela Our World in Data (OWID) é feita, mostrando o avanço temporal de casos, óbitos e vacinação. Estudos comparativos e busca de correlações são feitos com intuito de melhor compreender os dados. Animações como a mostrada acima serão criadas e interpretadas.

## Obtenção dos dados


<center><img alt="covid_banner" width="10%" src="https://ourworldindata.org/uploads/2019/02/OurWorldinData-logo.png"></center>

Como já citado, os dados aqui utilizados são fornecidos pela *Our World in Data* (OWID) [neste repositório](https://github.com/owid/covid-19-data), uma organização que reúne pesquisadores de todo o mundo para agregar dados de diversas fontes sobre os mais diversos problemas contemporâneos.

Obviamente, por ser uma doença recente, cuidados devem ser tomados no que diz respeito à interpretação dos dados e relações de causa e efeito. Como será mostrado adiante, nem todos os países fornecem dados completos e há algumas inconsistências nos dados a depender de suas origens. Além disso, com o crescente conhecimento a respeito da doença, por vezes se faz necessário alterar a forma de agregar os dados, causando algumas dificuldades de análise temporal. Não podemos ignorar também o fato de que, infelizmente, a visão passada por veículos de mídia e governantes para população pode variar e ser distorcida a depender dos interesses momentâneos dos mesmos.

Assim, durante esse estudo, especial cuidado é destinado em apresentar os dados e buscar interpretá-los de forma rigorosa deixando claro eventuais limitações, conflitos e condições de contorno de cada análise. E, sempre lembrando, correlações não implicam em causalidade.

A base de dados da OWID apresenta 67 variáveis (colunas) e 187.097 entradas. Há ausência de muitos dados sobre hospitalização e realização de testes por nem todos os países terem informações oficiais, de forma que essas categorias foram deixadas de fora desta análise.

O estudo utilizou a linguagem de programação Python e alguns pacotes para esta linguagem. O pacote [Pandas](https://pandas.pydata.org/) foi utilizado para trabalhar com os dados e construir matrizes de correlações. Os pacotes [Matplotlib](https://matplotlib.org/), [Seaborn](https://seaborn.pydata.org/) e [Plotly](https://plotly.com/) foram utilizados para construir os gráficos. Funções foram criadas para padronizar os estilos de cada tipo de gráfico utilizado no trabalho. Tais funções se encontram no arquivo `plots.py` no [repositório do estudo](https://github.com/chicolucio/panorama-covid-mundo).  A biblioteca `datetime`, presente em uma instalação padrão do Python, foi utilizada para lidar com datas. 

A base de dados baixada apresenta dados até 17 de maio de 2022, quando foi obtida. Porém, há alguns dados cuja periodicidade é diferenciada. Desta forma, vamos considerar como última data para análise *30 de abril de 2022* para termos apenas meses fechados, *de janeiro de 2020 até abril de 2022*.


## Evolução do número de casos

<center><img alt="covid_banner" width="50%" src="https://image.freepik.com/free-photo/3d-medical-grunge-background-with-abstract-coronavirus-cells_1048-11987.jpg"></center>

Vamos começar nossa análise avaliando a evolução no número total de casos em todo o mundo.

### Total de casos

#### Mundo

Muito do que é exposto nas mídias foca na quantidade total de casos por país ou mundialmente. Vamos avaliar o perfil de crescimento no número de casos mundial:


    
![png](../projeto_covid_files_april_2022/projeto_covid_21_0.png)
    


Vemos que, desde o surgimento em meados de janeiro de 2020, foram necessários cerca de 3 meses para atingir 1 milhão de casos e 1 ano para 100 milhões de casos. Lembrando que esses são os casos efetivamente computados devido a testagem ou procura das pessoas pelas redes de saúde. O número real é potencialmente maior tendo em vista que há casos brandos ou assintomáticos, de maneira que os indivíduos não procuram as redes de saúde. De qualquer forma, considerando que a população mundial é estimada na ordem de 7,8 bilhões de pessoas, **o número de casos até o momento indica que cerca de 6 % da população mundial já teve a doença**.

Muito se fala das variantes do vírus. A Organização Mundial da Saúde (OMS) em [seu site](https://www.who.int/en/activities/tracking-SARS-CoV-2-variants/) descreve os critérios para considerar uma determinada variante como sendo de interesse ou preocupante. A OMS também esclarece que os nomes são dados um certo tempo depois que as primeiras amostras de cada variante são analisadas, conforme a tabela:

| Variante | Data da primeira amostra | Data do nome |
|:------|:--------|:--------|
| alfa | Setembro/2020 - Reino Unido | Dezembro de 2020 
| beta | Maio/2020 - África do Sul | Dezembro de 2020 
| gama | Novembro/2020 - Brasil | Janeiro de 2021
| delta | Outubro/2020 - Índia | Abril de 2021
| lambda | Dezembro/2020 - Peru | Junho de 2021
| mu | Janeiro/2021 - Colômbia | Agosto de 2021
| ômicron | Novembro/2021 - Diversos | Novembro de 2021

Neste estudo, *optou-se por utilizar a data da primeira amostra nos gráficos*, motivo pelo qual a beta aparece antes da alfa. Tal escolha se deve ao fato de a variante já estar se espalhando desde a data de amostra independentemente de ter recebido um nome. Vamos adicionar aos gráficos anteriores linhas indicando as datas de aparecimento de cada variante:


    
![png](../projeto_covid_files_april_2022/projeto_covid_24_0.png)
    


O número de casos teve um acelerado crescimento inicial antes do surgimento da primeira variante (beta) e houve um aumento de 10 vezes entre as datas da variante beta e da variante alfa, um intervalo de 4 meses. O crescimento se torna, então, um mais lento. Nos quatro meses entre as variantes alfa e mu o número de casos cresceu 3 vezes.

Cerca de 2 meses depois do surgimento da variante ômicron, ou seja, em janeiro de 2022, houve um acentuado aumento no número de casos.

#### Principais países

Vamos agora olhar os países com mais casos até abril de 2022. 

##### Totais absolutos

Em um primeiro momento, podemos supor que países com maiores populações apresentem mais casos. Vamos verificar as maiores populações mundiais, e seus totais de casos e mortes:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>population</th>
      <th>total_cases</th>
      <th>total_deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>China</td>
      <td>1,444,216,102</td>
      <td>977,966</td>
      <td>5,060</td>
    </tr>
    <tr>
      <td>India</td>
      <td>1,393,409,033</td>
      <td>43,079,188</td>
      <td>523,843</td>
    </tr>
    <tr>
      <td>United States</td>
      <td>332,915,074</td>
      <td>81,352,100</td>
      <td>993,665</td>
    </tr>
    <tr>
      <td>Indonesia</td>
      <td>276,361,788</td>
      <td>6,046,796</td>
      <td>156,257</td>
    </tr>
    <tr>
      <td>Pakistan</td>
      <td>225,199,929</td>
      <td>1,527,956</td>
      <td>30,369</td>
    </tr>
    <tr>
      <td>Brazil</td>
      <td>213,993,441</td>
      <td>30,448,236</td>
      <td>663,736</td>
    </tr>
    <tr>
      <td>Nigeria</td>
      <td>211,400,704</td>
      <td>255,716</td>
      <td>3,143</td>
    </tr>
    <tr>
      <td>Bangladesh</td>
      <td>166,303,494</td>
      <td>1,952,691</td>
      <td>29,127</td>
    </tr>
    <tr>
      <td>Russia</td>
      <td>145,912,022</td>
      <td>17,917,191</td>
      <td>368,319</td>
    </tr>
    <tr>
      <td>Mexico</td>
      <td>130,262,220</td>
      <td>5,739,680</td>
      <td>324,334</td>
    </tr>
  </tbody>
</table>


Agora, vamos organizar uma tabela mostrando os países com mais casos e, também, expressar isto graficamente:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>continent</th>
      <th>population</th>
      <th>total_cases</th>
      <th>total_deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>United States</td>
      <td>North America</td>
      <td>332,915,074</td>
      <td>81,352,100</td>
      <td>993,665</td>
    </tr>
    <tr>
      <td>India</td>
      <td>Asia</td>
      <td>1,393,409,033</td>
      <td>43,079,188</td>
      <td>523,843</td>
    </tr>
    <tr>
      <td>Brazil</td>
      <td>South America</td>
      <td>213,993,441</td>
      <td>30,448,236</td>
      <td>663,736</td>
    </tr>
    <tr>
      <td>France</td>
      <td>Europe</td>
      <td>67,422,000</td>
      <td>28,699,367</td>
      <td>145,999</td>
    </tr>
    <tr>
      <td>Germany</td>
      <td>Europe</td>
      <td>83,900,471</td>
      <td>24,809,785</td>
      <td>135,461</td>
    </tr>
    <tr>
      <td>United Kingdom</td>
      <td>Europe</td>
      <td>68,207,114</td>
      <td>22,112,691</td>
      <td>175,081</td>
    </tr>
    <tr>
      <td>Russia</td>
      <td>Europe</td>
      <td>145,912,022</td>
      <td>17,917,191</td>
      <td>368,319</td>
    </tr>
    <tr>
      <td>South Korea</td>
      <td>Asia</td>
      <td>51,305,184</td>
      <td>17,275,649</td>
      <td>22,875</td>
    </tr>
    <tr>
      <td>Italy</td>
      <td>Europe</td>
      <td>60,367,471</td>
      <td>16,463,200</td>
      <td>163,507</td>
    </tr>
    <tr>
      <td>Turkey</td>
      <td>Asia</td>
      <td>85,042,736</td>
      <td>15,032,093</td>
      <td>98,771</td>
    </tr>
  </tbody>
</table>



    
![png](../projeto_covid_files_april_2022/projeto_covid_40_0.png)
    


Vemos que 4 países dentre os que apresentam maiores populações no mundo também figuram entre os com mais casos: Estados Unidos, Índia, Brasil e Rússia. Há a ausência da China, mas os dados de lá não são confiáveis tendo em vista o regime ditatorial, sendo que o país até mesmo [se nega a fornecer](https://www.forbes.com/sites/jemimamcevoy/2021/07/15/who-chief-china-not-sharing-critical-data-in-covid-origins-probe/?sh=411fe3e47591) dados mais completos sobre a própria origem do vírus.

Outra ausência que chama a atenção é a Indonésia. Novamente, devido a questões políticas, não se pode confiar totalmente nos dados reportados por tal país, visto que o [próprio presidente da Indonésia](https://www.thejakartapost.com/news/2020/03/13/we-dont-want-people-to-panic-jokowi-says-on-lack-of-transparency-about-covid-cases.html) afirmou em dado ponto que não estavam disponibilizando informações para "evitar pânico" na população. Obviamente, tal posicionamento é [amplamente criticado](https://indonesiaatmelbourne.unimelb.edu.au/talking-indonesia-covid-19-and-data-transparency/).

Vemos por esses dois casos que há diversos fatores sociais, econômicos e políticos que podem afetar a análise.  Há países entre as maiores populações que enfrentam questões políticas, econômicas e de saúde complicadas, como Paquistão, Bangladesh e Nigéria, que podem afetar a capacidade de tais países em acompanhar e reportar os casos adequadamente. Inclusive, de acordo com [dados e relatórios das Nações Unidas](https://unstats.un.org/unsd/demographic-social/crvs/#coverage), apenas dois terços dos países do mundo possuem registros de pelo menos 90 % dos nascimentos e óbitos. Todos os países citados aparecem nos relatórios como países com dificuldades de registro.

Neste estudo, vamos nos ater ao que é apresentado. Mas é importante notar tais situações. 

Comparando com a [primeira versão deste estudo](https://franciscobustamante.com.br/portfolio/2021-10-projeto_covid/), publicada em outubro de 2021, temos as três primeiras posições com os mesmos países. A França passou da sétima posição para a quarta, anteriormente ocupada pelo Reino Unido. A Alemanha nem ao menos aparecia na lista em outubro de 2021 e, agora, ocupa a quinta posição, antes ocupada pela Rússia. A Túrquia antes ocupava a sexta posição e, agora, a décima. Itália e Coreia do Sul não figuravam na lista.  

Vamos montar um gráfico interativo para avaliar cada país por continente.

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/33.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique nos continentes para ter mais 
detalhes do mesmo. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>

No gráfico, temos uma ideia da proporção do total de casos de cada país frente aos demais de seu continente e do mundo.

Mas será que essa é a melhor métrica para avaliar comparativamente a situação de cada país?

##### Totais por milhão de habitantes

Uma análise do total absoluto de casos não necessariamente reflete a realidade de cada país. Afinal, como já dito, países com maiores populações tendem a ter maior número de casos. No entanto, isso não significa que países com populações pequenas não possam estar em situação pior. Afinal, podem, relativamente ao seu número de habitantes e à sua estrutura de saúde pública, estar com elevado número de casos. E regiões com maiores densidades populacionais podem favorecer uma maior taxa de transmissão (hipótese que será estudada mais adiante).

Vamos, então, verificar os países com mais casos por milhão de habitantes:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>continent</th>
      <th>population</th>
      <th>total_cases</th>
      <th>total_cases_per_million</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Faeroe Islands</td>
      <td>Europe</td>
      <td>49,053.00</td>
      <td>34,658.00</td>
      <td>706,541.90</td>
    </tr>
    <tr>
      <td>Denmark</td>
      <td>Europe</td>
      <td>5,813,302.00</td>
      <td>3,116,655.00</td>
      <td>536,124.74</td>
    </tr>
    <tr>
      <td>Cyprus</td>
      <td>Europe</td>
      <td>896,005.00</td>
      <td>480,220.00</td>
      <td>535,956.83</td>
    </tr>
    <tr>
      <td>Andorra</td>
      <td>Europe</td>
      <td>77,354.00</td>
      <td>41,349.00</td>
      <td>534,542.49</td>
    </tr>
    <tr>
      <td>Gibraltar</td>
      <td>Europe</td>
      <td>33,691.00</td>
      <td>17,837.00</td>
      <td>529,429.22</td>
    </tr>
    <tr>
      <td>Iceland</td>
      <td>Europe</td>
      <td>368,792.00</td>
      <td>185,579.00</td>
      <td>503,207.77</td>
    </tr>
    <tr>
      <td>Slovenia</td>
      <td>Europe</td>
      <td>2,078,723.00</td>
      <td>1,010,058.00</td>
      <td>485,903.12</td>
    </tr>
    <tr>
      <td>San Marino</td>
      <td>Europe</td>
      <td>34,010.00</td>
      <td>16,437.00</td>
      <td>483,299.03</td>
    </tr>
    <tr>
      <td>Netherlands</td>
      <td>Europe</td>
      <td>17,173,094.00</td>
      <td>8,141,249.00</td>
      <td>474,070.02</td>
    </tr>
    <tr>
      <td>Saint Pierre and Miquelon</td>
      <td>North America</td>
      <td>5,771.00</td>
      <td>2,701.00</td>
      <td>468,029.80</td>
    </tr>
  </tbody>
</table>



    
![png](../projeto_covid_files_april_2022/projeto_covid_49_0.png)
    


Percebemos uma grande mudança, aparecendo pequenos territórios e países. Na primeira posição, temos as Ilhas Faroe, um [território dependente da Dinamarca](https://en.wikipedia.org/wiki/Faroe_Islands) com cerca de 50 mil habitantes. Por mais que a localidade ainda seja oficialmente parte da Dinamarca, a base de dados em questão trabalha com o conceito de territórios e não de países. 

A própria Dinamarca, considerando a [porção continental](https://en.wikipedia.org/wiki/Danish_Realm) do país, aparece em segundo. Ainda no início de 2021, o país já enfrentava críticas internas sobre sua política de lockdown [como mostra esse artigo da Science](https://www.science.org/content/article/danish-scientists-see-tough-times-ahead-they-watch-more-contagious-covid-19-virus-surge). Recentemente, no início de 2022, o país retirou todas as restrições e [viu o número de casos diários diminuir significativamente](https://www.thelocal.dk/20220425/covid-19-denmark-registers-under-1000-cases-in-a-day-for-first-time-in-2022/). No entanto, como descrito no artigo citado, a política de testagem mudou, não sendo possível fazer uma comparação adequada entre o período atual, sem restrições, e o anterior, com restrições. Em [recente entrevista](https://youtu.be/1s1UX2ZNsh4), Camilla Holten-Moller, uma das responsáveis por modelos matemáticos utilizados pelo governo dinamarquês, justificou a retirada de todas as restrições ao final de janeiro de 2022 ainda que os casos na época estivessem aumentando devido à variante ômicron. E ainda fez análises críticas sobre o uso de modelos matemáticos em situações políticas.

Outros territórios aparecem na listagem:

- Gibraltar (território britânico)
- São Pedro e Miquelão (território francês), único da lista não localizado na Europa

Como vemos, quando consideramos a proporção de casos ao invés do total absoluto, o perfil da análise muda significativamente. E fica mais seguro e coerente fazer comparações com os dados em uma mesma escala. O fato de termos predominância de localidades na Europa sugere que há características do continente que favorecem o espalhamento do vírus. Tais características podem ser: clima, idade da população, densidade populacional, dentre inúmeras outras. Adiante, iremos nos aprofundar em algumas dessas características e verificar se há algum tipo de correlação com os dados de COVID.

Apenas três localidades da listagem possuem populações na ordem milhões: Dinamarca, Eslovênia e Holanda. Sobre a Dinamarca já escrevemos um pouco anteriormente. Tanto a [Eslovênia](https://en.wikipedia.org/wiki/COVID-19_pandemic_in_Slovenia) quanto a [Holanda](https://nltimes.nl/2022/05/16/netherlands-lack-covid-strategy-result-lockdowns-report) também adotaram políticas de lockdown e restrições, que foram retiradas nos últimos meses. Como descrito no link sobre a Holanda, o país já enfrentava dificuldades no setor de saúde antes da pandemia, por falta de profissionais, e há críticas quanto ao modo que o governo local abordou a situação durante a pandemia e preocupação caso haja uma nova onda da doença. Ainda assim, o número de casos no país cai continuamente após a retirada das restrições.

Vamos criar um gráfico interativo mostrando cada país por continente:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/35.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique nos continentes para ter mais 
detalhes do mesmo. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>

Veja como a análise proporcional muda significativamente o perfil do gráfico. Todos os países que apareciam como líderes em totais absolutos não figuram entre os que possuem mais casos por milhão de habitantes. Na Europa, vemos uma maior participação de países do leste europeu que tiveram [uma nova onda](https://edition.cnn.com/2021/10/25/uk/europe-covid-second-pandemic-winter-intl-gbr/index.html) ao final do ano passado. Mesmo com os casos na região caindo, a [OMS tem alertado os países da região](https://www.reuters.com/business/healthcare-pharmaceuticals/omicron-threat-remains-high-east-europe-who-2022-02-15/) sobre o risco de novas ondas. Na América do Sul, vemos que o Brasil se encontra em quarto, atrás de Argentina, Uruguai e Chile. Uruguai e Argentina [foram considerados modelos](https://www.nexojornal.com.br/expresso/2021/04/15/As-respostas-de-Argentina-e-Uruguai-aos-recordes-de-covid) em diversos estágios da pandemia com suas medidas restritivas e lockdowns, mas agora lideram o total de casos por milhão.

Na Ásia, vemos que o país com mais casos por milhão de habitantes é Israel. O país adotou lockdowns e diversas estratégias restritivas e foi um dos mais ágeis do mundo na distribuição de vacinas, inclusive as doses de reforço, para população. No entanto, como vemos, os resultados não foram os esperados. Em [recente entrevista](https://youtu.be/bnMMYJKZvnU), Cyrille Cohen, um dos membros do comitê de vacinas do governo israelense, disse "cometemos enganos" durante a pandemia. Dentre os "enganos" ele cita: passaportes vacinais, que não são efetivos; as vacinas, que não previnem transmissão como esperado inicialmente, e que não deveriam ser obrigatórias; ter fechado escolas e universidades, o que prejudicará de forma incalculável a evolução cognitiva de crianças e jovens. Por fim, na entrevista, Cohen afirma que não se deveria ignorar a imunidade de rebanho e que, com a ômicron e novas variantes, a COVID será "como uma gripe".

Vamos construir uma mapa interativo mostrando a evolução do total de casos por milhão em cada país. A consolidação foi feita por mês apenas para deixar a animação do mapa mais rápida, se fosse evolução diária seria uma animação bem demorada.

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/37.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique na seta de play. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>


Vemos no mapa que apenas alguns países da África apresentaram significativa evolução no número de casos. Como já escrito anteriormente, provavelmente isso está relacionado com as precárias condições de tais países em reportar adequadamente casos e mesmo realizar testes. No entanto, como será mostrado adiante, o perfil mais jovem da população também pode ter alguma relação, já que a doença é mais branda em jovens, o que pode fazer com que menos pessoas busquem tratamento, ficando de fora dos números oficiais. Há [estudos](https://www.sciencedirect.com/science/article/pii/S120197122032172X) que sugerem tal explicação, além de outras. Mais algumas referências sobre o assunto COVID na África: [Nature](https://www.nature.com/articles/s41577-021-00579-y), [JNMA](https://www.sciencedirect.com/science/article/pii/S002796842030345X) e [Science](https://www.science.org/doi/full/10.1126/science.abf8832). Todas as referências citam o fato de o número de casos e mortes no continente africano estarem bem abaixo do esperado com base em modelos e que ainda não há clareza sobre os motivos.

Na Oceania, vemos poucos casos na Austrália e na Nova Zelândia até o início de 2022. Os dois países adotaram políticas altamente restritivas e questionáveis, como [campos de quarentena forçada](https://youtu.be/mGFdWcJU7-0), buscando zerar novos casos. Agora, se [veem forçados a afrouxar](https://www.bloomberg.com/news/articles/2021-08-22/n-z-says-delta-raises-questions-about-its-covid-zero-approach) tais políticas por não terem tido o efeito desejado e estarem quebrando suas economias. Ao reabrirem suas economias pelo fracasso da política de COVID zero, viram o números de casos disparar, especialmente pela variante ômicron.

Na Austrália, a [população já estava saturada](https://www.theguardian.com/world/2022/may/07/explainer-why-are-covid-infection-rates-in-australia-so-high-compared-to-other-countries) das restrições e o número particularmente alto também pode ser reflexo dos testes de PCR ainda serem gratuitos, o que não é o caso em boa parte dos países. Na Nova Zelândia, a disruptura social causada pelas medidas restritivas tem levado a [um aumento na criminalidade em grandes centros](https://www.theguardian.com/world/2022/may/15/after-covid-swells-in-new-zealands-empty-city-centres) após a reabertura. A preocupação é a maior participação de jovens em atividades criminosas, tendo em vista que muitos se afastaram das escolas e colégios durante os lockdowns. Além da situação financeira, com o aumento do custo de vida, que tem gerado problemas de habitação.

Observe no parágrafo anterior o comentário sobre os testes de PCR. Já havíamos visto que a Dinamarca mudou sua política de testagem, apenas para ["casos médicos especiais"](https://www.thelocal.dk/20220310/denmark-says-covid-19-testing-now-only-needed-for-special-medical-reasons/), enquanto que a Austrália ainda faz testagem gratuitamente em sua população. Estas diferentes políticas adotadas por países dificulta a comparação entre os mesmos e pode levar a distorções nos dados. Inclusive, o primeiro ministro australiano Scott Morrison, candidato à reeleição que [foi derrotado](https://www.france24.com/en/australia/20220521-australia-s-conservative-pm-scott-morrison-concedes-election-defeat) nas eleições que ocorreram em maio de 2022, criticou a [política de testagem atual e de registro de óbitos em recente entrevista](https://www.ndtv.com/world-news/australia-coronavirus-australia-covid-covid-in-australia-why-australias-covid-cases-are-rising-again-2986729). Com mais de 95 % da população adulta vacinada, Morrison disse que se deve conviver com a COVID, e que muitas mortes registradas com COVID não necessariamente foram devido à COVID. Isto porque os protocolos locais orientam o registro como COVID de qualquer situação confirmada ou que seja provável de infecção pelo vírus, a menos que uma "clara alternativa" exista.

#### Idade e riqueza

Vamos verificar se o número de casos possui alguma relação com a renda per capita do país ou a idade da população.

Comecemos verificando os países com maior PIB per capita:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>population</th>
      <th>total_cases</th>
      <th>total_cases_per_million</th>
      <th>gdp_per_capita</th>
      <th>continent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Qatar</td>
      <td>2,930,524.00</td>
      <td>364,602.00</td>
      <td>124,415.29</td>
      <td>116,935.60</td>
      <td>Asia</td>
    </tr>
    <tr>
      <td>Macao</td>
      <td>658,391.00</td>
      <td>82.00</td>
      <td>124.55</td>
      <td>104,861.85</td>
      <td>Asia</td>
    </tr>
    <tr>
      <td>Luxembourg</td>
      <td>634,814.00</td>
      <td>237,856.00</td>
      <td>374,686.13</td>
      <td>94,277.96</td>
      <td>Europe</td>
    </tr>
    <tr>
      <td>Singapore</td>
      <td>5,453,600.00</td>
      <td>1,193,250.00</td>
      <td>218,800.42</td>
      <td>85,535.38</td>
      <td>Asia</td>
    </tr>
    <tr>
      <td>Brunei</td>
      <td>441,532.00</td>
      <td>141,850.00</td>
      <td>321,267.77</td>
      <td>71,809.25</td>
      <td>Asia</td>
    </tr>
    <tr>
      <td>Ireland</td>
      <td>4,982,904.00</td>
      <td>1,517,112.00</td>
      <td>304,463.42</td>
      <td>67,335.29</td>
      <td>Europe</td>
    </tr>
    <tr>
      <td>United Arab Emirates</td>
      <td>9,991,083.00</td>
      <td>898,571.00</td>
      <td>89,937.30</td>
      <td>67,293.48</td>
      <td>Asia</td>
    </tr>
    <tr>
      <td>Kuwait</td>
      <td>4,328,553.00</td>
      <td>631,409.00</td>
      <td>145,870.69</td>
      <td>65,530.54</td>
      <td>Asia</td>
    </tr>
    <tr>
      <td>Norway</td>
      <td>5,465,629.00</td>
      <td>1,426,013.00</td>
      <td>260,905.56</td>
      <td>64,800.06</td>
      <td>Europe</td>
    </tr>
    <tr>
      <td>Switzerland</td>
      <td>8,715,494.00</td>
      <td>3,619,598.00</td>
      <td>415,306.12</td>
      <td>57,410.17</td>
      <td>Europe</td>
    </tr>
  </tbody>
</table>


Como era de se esperar, vemos apenas países da Ásia e Europa. 

A base de dados possui entradas para agregados de países com base na classificação de renda do Banco Mundial. Tal classificação baseia-se na renda per capita da seguinte forma (dólares americanos):


| Classifição | Limite inferior | Limite superior
| --- | --- | --- 
| Renda baixa (*low-income*) | - | 1045
| Renda média baixa (*lower-middle-income*) | 1046 | 4095
| Renda média alta (*upper-middle-income*) | 4096 | 12695
| Renda alta (*high-income*) | 12696 | -

Para detalhes de classificação de cada país, [veja este link](https://datahelpdesk.worldbank.org/knowledgebase/articles/906519-world-bank-country-and-lending-groups). O Brasil, por exemplo, é considerado uma economia de renda média alta. Abordaremos com mais detalhes o caso brasileiro em uma seção específica mais adiante. 

Vejamos, então, o total de casos por milhão de habitantes considerando estes agregados:


    
![png](../projeto_covid_files_april_2022/projeto_covid_60_0.png)
    


Vemos claramente que há mais casos em países de renda alta, com quase duas ordens de grandeza mais casos que países de renda baixa. O perfil de crescimento é similar em todas as classificações.

Vamos criar um gráfico interativo buscando avaliar como os casos variam com a expectativa de vida e o PIB per capita:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/39.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, passe o mouse pelos círculos ou clique nos nomes dos continentes para mostrá-los ou não. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>


No gráfico vemos claramente que os países do continente africano possuem, em sua maioria, menores expectativas de vida e menores PIB per capita (menores círculos no gráfico). No entanto, Seicheles, país africano com maior PIB per capita e uma das maiores expectativas de vida do continente, lidera o continente em total de casos por milhão. [Na primeira versão deste trabalho](https://franciscobustamante.com.br/portfolio/2021-10-projeto_covid/), em outubro de 2021, Seicheles liderava o mundo em casos por milhão, mesmo sendo o país com maior porcentagem da população vacinada à época.

Vemos uma tendência similar em outros continentes, maiores expectativas de vida e PIB per capita com mais casos. Obviamente que países mais ricos tendem a ter uma maior parcela da população com idade avançada e, como a doença afeta mais pessoas idosas, é de se esperar maior notificação de casos nesses países. No entanto, cabe destacar que há notáveis exceções à essa tendência como, por exemplo, o Japão.

Uma maior expectativa de vida, no entanto, não necessariamente significa que a população atual possui mais idosos que jovens. Assim, vamos repetir o gráfico substituindo o eixo de expectativa de vida por mediana de idade:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/41.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, passe o mouse pelos círculos ou clique nos nomes dos continentes para mostrá-los ou não. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>


Vemos a mesma tendência geral: população mais velha, mais casos notificados. É interessante notar que o Japão possui a maior mediana de idade do mundo e não possui um valor muito elevado de casos por milhão. O país não adotou políticas como lockdowns e quarentenas compulsórias pois entendeu que tais medidas impostas violariam os direitos humanos expressos em sua constituição. Ao invés, adotou o sistema de [estados de emergência](https://en.wikipedia.org/wiki/COVID-19_pandemic_in_Japan#State_of_Emergency), onde apenas algumas medidas voltadas para locais com muitas pessoas podiam ser tomadas e, mesmo assim, não havia poder legal de impedir a movimentação de pessoas. Inclusive, a [Corte de Tóquio considerou ilegal](https://www.japantimes.co.jp/news/2022/05/16/national/crime-legal/japan-covid-measures-illegal-ruling/) uma ordem do governo metropolitano de restringir o horário de uma cadeia de restaurantes.

Enquanto a política interna não foi muito restritiva, a voltada para as fronteiras foi. Apenas agora, em maio de 2022, o país [começa a abrir aos poucos](https://www.theguardian.com/world/2022/may/17/japan-prepares-to-reopen-to-tourists-for-first-time-since-2020) suas fronteiras, ainda com diversas restrições. O Japão é o único país do G7 que ainda não retirou restrições de viagem e sua política de fronteiras tem recebido críticas como ["isolacionista e xenófoba"](https://thediplomat.com/2022/05/whats-behind-japans-continued-covid-19-border-restrictions/), além de sem sentido tendo em vista a transmissibilidade da variante ômicron. A pressão interna para reabertura tem aumentado, já que a [economia do país passa por dificuldades](https://www.nytimes.com/2022/05/17/business/japan-economy-covid.html) e o turismo sempre foi uma grande fonte de recursos. A guerra da Ucrânia e os [absurdos lockdowns na China](https://www.nbcnews.com/news/world/shanghai-videos-residents-clash-police-china-covid-protest-rcna24520) causaram aumento de preços dentro do Japão por ruptura da cadeia de suprimentos.

Mas se o Japão não adotou medidas restritivas severas como, então, não teve um grande número de casos e de óbitos? Ainda no início da pandemia, [estudos](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7207161/) tentavam responder esse questionamento. Alguns aspectos culturais foram considerados, como a ausência de apertos de mão e abraços ao cumprimentar, a já presente [cultura de uso de máscaras](https://www.japanesestudies.org.uk/ejcjs/vol14/iss2/horii.html), alguns aspectos genéticos e uma possível ajuda da vacina BCG, obrigatória em crianças. Nenhuma dessas alternativas provavelmente responde por completo a questão. As máscaras, por exemplo, são utilizadas por pessoas de diversos países na Ásia para evitar rinite por pólen ou diminuir o efeito respiratório da poluição de grandes centros. Além disso, há [estudos](https://www.nejm.org/doi/full/10.1056/NEJMp2006372) que mostram que [máscaras são pouco efetivas](https://wwwnc.cdc.gov/eid/article/26/5/19-0994_article) para proteger contra infeção respiratória.

Talvez uma explicação esteja no [sistema de saúde](https://en.wikipedia.org/wiki/Health_care_system_in_Japan) e a experiência do país em outras epidemias respiratórias. A maneira como o país [monitora casos de influenza](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5155020/) é considerada modelo e [há indícios](https://onlinelibrary.wiley.com/doi/10.1111/irv.12977) que o protocolo utilizado, e que foi adotado inicialmente para a pandemia, tenha colaborado no tratamento nos estágios iniciais de COVID.

Por fim, é interessante notar como a Itália, com expectativa de vida, mediana de idade, e PIB per capita similares aos do Japão, possui cerca de 4 vezes mais casos. Além disso, a população da Itália é de cerca de 60 milhões de habitantes, pouco menos da metade dos 125 milhões do Japão, de forma que a densidade populacional italiana é bem menor. 

Lembrando, [a Itália foi o país europeu mais afetado](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7767319/) no início da pandemia. Em março de 2020, era o terceiro em total de casos e o primeiro em número de óbitos em todo o mundo. Na época, muito se falou sobre a idade da população, mas, como vemos agora, não se pode avaliar um único parâmetro já que, por idade, o Japão deveria ter dados similares. 

Há muito mais a ser analisado e nem sempre é possível resumir tudo em números que representam bem todo um país. 

[Este artigo](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7908106/) avalia como as 21 regiões administrativas italianas lidaram com o início da pandemia. Na Itália, cada região tem certa autonomia para organizar suas políticas de saúde. O artigo mostra como as regiões que adotaram hospitalização logo no início tiveram números piores do que outras regiões. Além disso, destaca como diferentes políticas de testagem dificultam a comparação de casos entre as regiões, um aspecto já levantado em outros contextos aqui neste estudo. Por fim, o artigo também destaca que cerca de um terço das mortes atribuídas ao COVID-19 até maio de 2020 ocorreram em casas de repouso para idosos e que não necessariamente este número é confiável.

### Novos casos

Até o momento focamos em avaliar a evolução do total de casos no mundo e em cada país. Agora, vamos verificar a evolução de novos casos, à medida que foram sendo reportados. Assim, podemos verificar em quais momentos houve maior notificação, as popularmente chamadas "ondas" de casos.

A base de dados apresenta as entradas na forma de dados diários e na forma de média móvel de 7 dias. A média móvel ajuda a visualizar melhor a tendência dos dados, visto que há grande variação dos dados diários. Vamos começar com os casos em todo mundo:


    
![png](../projeto_covid_files_april_2022/projeto_covid_68_0.png)
    


Vemos que está surgindo um padrão de aceleração de casos e máximos. Em 2020, vimos três grandes saltos em novos casos, com inícios em março, em junho e em novembro. Em 2021, vimos novamente saltos em março e junho. Na virada de 2021 para 2022 tivemos um grande aumento de casos.

Podemos verificar se há algum indício de relação entre novos casos e as variantes:


    
![png](../projeto_covid_files_april_2022/projeto_covid_70_0.png)
    


Em 2020, vemos que o aumento de casos entre junho e julho ocorre após o surgimento da variante beta. De forma similar, após o surgimento da variante delta, ocorre o aumento significativo de casos entre outubro e novembro. Isso corrobora as afirmações veiculadas à época na mídia que tal variante era mais transmissível que as primeiras. Durante o ano de 2021, algumas variantes de menor impacto surgiram, mas ficaram pouco tempo como variantes de interesse na classificação da OMS. Em novembro de 2021, houve o surgimento da variante ômicron e, logo após sua identificação, o grande aumento no número de casos na passagem de 2021 para 2022.

Agora, sabemos que em estações mais frias usualmente há [mais casos de resfriados e gripes](https://www.cdc.gov/flu/about/season/flu-season.htm). Será que há alguma relação entre inverno e casos de COVID-19? Vejamos:


    
![png](../projeto_covid_files_april_2022/projeto_covid_72_0.png)
    


Vemos que há uma possível relação, repare os períodos entre junho e setembro dos anos de 2020 e 2021, e os períodos entre dezembro e março nas passagens de 2020 para 2021 e de 2021 para 2022. No entanto, o máximo ao final de abril de 2021 não encaixa nessa explicação. Como veremos adiante, este máximo é devido a casos na Índia, sendo devido a situações específicas deste país. Assim, podemos considerar que há uma forte correlação entre períodos de frio e casos de COVID, o que é característico de infecções respiratórias. 

Vamos comparar os dados consolidados mundialmente com os dados individuais dos países que vimos anteriormente possuir mais casos:


    
![png](../projeto_covid_files_april_2022/projeto_covid_74_0.png)
    


Aqui vemos mais claramente como o pico entre abril e maio de 2021 possui grande influência da Índia. Na época, [notícias](https://www.nature.com/articles/d41586-021-01059-y) também demonstravam uma certa dúvida nos motivos que levaram a Índia a ter esse surto de casos, suspeitando-se de novas variantes ou de espalhamento para os grandes centros urbanos. Posteriormente, ficou mais [evidente o papel das eleições que ocorreram em algumas localidades](https://thewire.in/politics/election-rally-covid-19-case-spike). Também contribuiu o fato de milhares de pessoas, com medo do aumento de casos, recorrerem ao [ritual de se dirigir ao rio Ganges](https://www.reuters.com/business/healthcare-pharmaceuticals/india-overtakes-brazil-worlds-second-worst-hit-country-by-covid-19-2021-04-12/) para se banhar, já que o rio é considerado sagrado na cultura hindu.


A escala linear do gráfico não é muito adequada para uma comparação. Vamos recriar o gráfico com uma escala logarítmica:


    
![png](../projeto_covid_files_april_2022/projeto_covid_77_0.png)
    


Vemos que, com exceção do período entre abril e setembro de 2020, os comportamentos de Estados Unidos, França e Alemanha possuem perfis similares. As curvas de Índia e Brasil não se assemelham entre si nem com os demais países, exceto no que diz respeito aos novos casos decorrentes da variante ômicron no início de 2022.

Partimos agora para uma análise mais global.  Já vimos que aparentemente há um caráter sazonal nos casos de COVID-19. Vamos criar um mapa que apresente esse perfil para cada país do mundo:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/43.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique na seta de play. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>



Observe como as cores oscilam para os países enquanto o tempo passa, e como as oscilações não são sincronizadas. Compare, por exemplo, os Estados Unidos e o Brasil para perceber essa falta de sincronia. Será que conseguimos analisar esse comportamento a nível de continentes?

Vamos verificar o perfil de cada continente, ainda comparando com o acumulado mundial:


    
![png](../projeto_covid_files_april_2022/projeto_covid_84_0.png)
    


Aqui percebemos que o perfil da Ásia não acompanha fielmente o da Europa e o da América do Norte, mesmo estando todos estes continentes no mesmo hemisfério. Mas, como vimos, há grande efeito dos números da Índia no continente asiático e há diversos países suspeitos de subnotificação no mesmo continente, de forma que a comparação pode ser prejudicada.

Quanto as continentes do hemisfério sul, não se pode afirmar claramente se há alguma tendência conjunta. 

É perceptível no gráfico o efeito da variante ômicron na Oceania. Como já discutido anteriormente, a política de COVID zero adotada por Austrália e Nova Zelândia fracassou e os dois países vêm enfrentando um grande aumento de casos.

A dificuldade em encontrar padrões entre continentes é resultado da grande diferença de população entre os mesmos, além da posição geográfica e de possíveis subnotificações já citadas. Não necessariamente pertencer ao mesmo hemisfério significa ter estações do ano similares. Além de haver diversos fatores relacionados ao microclima de cada país/região e, obviamente, aspectos políticos e econômicos muito distintos. Apenas de forma ilustrativa, vejamos a população de cada continente e o total de casos por milhão de habitantes:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>total_cases_per_million</th>
      <th>population</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Oceania</td>
      <td>166,283</td>
      <td>43,219,954</td>
    </tr>
    <tr>
      <td>South America</td>
      <td>130,621</td>
      <td>434,260,137</td>
    </tr>
    <tr>
      <td>North America</td>
      <td>161,446</td>
      <td>596,581,283</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>257,362</td>
      <td>748,962,983</td>
    </tr>
    <tr>
      <td>Africa</td>
      <td>8,499</td>
      <td>1,373,486,472</td>
    </tr>
    <tr>
      <td>Asia</td>
      <td>31,825</td>
      <td>4,678,444,992</td>
    </tr>
  </tbody>
</table>


Veja como o total de casos por milhão é significativamente maior na Europa. O continente africano possui um número muito baixo de casos por milhão, conforme já discutido anteriormente. Quanto à Ásia, o valor baixo se deve a números não confiáveis de diversos países, pelos motivos vistos no início deste estudo, principalmente China e Indonésia. O caso da Oceania é um reflexo do grande número de casos da variante ômicron na Austrália e na Nova Zelândia.

Assim como já discutido para o total de casos, talvez seja mais elucidativo comparar os novos casos por milhão de habitantes, para diminuir eventuais efeitos de países com grandes populações. Comecemos mostrando o perfil de novos casos por milhão para os países que anteriormente vimos serem os que possuem maior total de casos até o fim de abril de 2022:


    
![png](../projeto_covid_files_april_2022/projeto_covid_90_0.png)
    


Aqui percebemos como França e Alemanha foram muito afetadas pela variante ômicron, na passagem do ano 2021 para 2022. Muito mais que o Brasil, por exemplo. Novamente, fica evidente a importância de se comparar números relativos, e não números absolutos, quando se compara países. Os Estados Unidos também apresentaram muitos novos casos. Como já mostrado anteriormente, o grande aumento no período coincide com o inverno no Hemisfério Norte.

Vejamos o gráfico de novos casos por milhão de habitantes para os continentes:


    
![png](../projeto_covid_files_april_2022/projeto_covid_93_0.png)
    


O gráfico está em escala logarítmica pois as ordens de grandeza mudam muito entre os continentes. Vemos como cada continente possui um comportamento próprio, tendo em vista que cada um possui países com especifidades próprias, conforme viemos discutindo durante todo o estudo. É perceptível que a variante ômicron foi significativa em todos os continentes.

Vejamos a evolução temporal de novos casos por milhão de habitantes através de um mapa:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/45.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique na seta de play. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>

Assim como já visto no mapa para o número absoluto de novos casos, fica evidente o caráter sazonal, popularmente conhecido como "ondas" de casos.

Vejamos como foi o comportamento de novos casos considerando agregados de países com base na classificação de renda do Banco Mundial:


    
![png](../projeto_covid_files_april_2022/projeto_covid_99_0.png)
    


Assim como já havíamos discutido para o total de casos, vemos que as ondas de novos casos possuem maior impacto em países de mais alta renda. A variante ômicron, na passagem de 2021 para 2022, é perceptível em todas as classificações.

Agora, será que há alguma forma de medir a velocidade de transmissão da doença?

### Taxa efetiva de reprodução (R)

A taxa efetiva de reprodução (R) é o número médio de pessoas infectadas em determinado momento por um indivíduo infectado introduzido em uma população *parcialmente imune*. Atenção ao "parcialmente imune", pois no início da pandemia muito se falava do R<sub>0</sub>, que é um conceito análogo mas para uma população completamente suscetível, ou seja, sem indivíduos imunizados seja por contágio prévio ou por vacinação.

A [interpretação](https://www.dw.com/pt-br/o-que-%C3%A9-o-n%C3%BAmero-de-reprodu%C3%A7%C3%A3o-r/a-53397119) do valor calculado de R é simples:

- R > 1: o número de casos da doença está aumentando. Epidemia.
- R = 1: cada infectado causa uma nova infecção. Endemia.
- R < 1: cada vez menos indivíduos se infectam e o número dos contágios retrocede.

Vamos verificar se podemos visualizar alguma relação entre mudanças no valor de R, as diversas variantes e períodos de inverno:


    
![png](../projeto_covid_files_april_2022/projeto_covid_103_0.png)
    


Valores elevados aparecem apenas no início do período. É possível perceber leves aumentos coincidentes com os períodos de máximos de novos casos discutidos anteriormente. Mesmo a variante ômicron, reconhecível no gráfico pelo máximo em janeiro de 2022, não atingiu um valor de *R* comparável ao início da pandemia. Como se trata dos valores de todo o mundo, a curva pode estar sendo balanceada entre países com taxas elevadas e países com taxas baixas. Vamos refazer o gráfico com os dados dos cinco países com mais casos:


    
![png](../projeto_covid_files_april_2022/projeto_covid_105_0.png)
    


Aqui vemos mais flutuações. Os períodos de maiores valores coincidem com os períodos de aumento de novos casos vistos anteriormente.

Em um primeiro momento, podemos pensar se há alguma relação entre a taxa de reprodução efetiva e a densidade populacional de um país. Afinal, mais pessoas próximas pode levar à uma maior transmissibilidade. Vamos verificar os países com maiores taxas de reprodução efetiva ao fim de abril de 2022 e verificar se há algum sinal de correlação:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>continent</th>
      <th>population</th>
      <th>population_density</th>
      <th>reproduction_rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Taiwan</td>
      <td>Asia</td>
      <td>23,855,008.00</td>
      <td>NaN</td>
      <td>2.19</td>
    </tr>
    <tr>
      <td>Grenada</td>
      <td>North America</td>
      <td>113,015.00</td>
      <td>317.13</td>
      <td>1.61</td>
    </tr>
    <tr>
      <td>Jamaica</td>
      <td>North America</td>
      <td>2,973,462.00</td>
      <td>266.88</td>
      <td>1.57</td>
    </tr>
    <tr>
      <td>Mauritania</td>
      <td>Africa</td>
      <td>4,775,110.00</td>
      <td>4.29</td>
      <td>1.57</td>
    </tr>
    <tr>
      <td>Panama</td>
      <td>North America</td>
      <td>4,381,583.00</td>
      <td>55.13</td>
      <td>1.55</td>
    </tr>
    <tr>
      <td>Saint Lucia</td>
      <td>North America</td>
      <td>184,401.00</td>
      <td>293.19</td>
      <td>1.53</td>
    </tr>
    <tr>
      <td>Eswatini</td>
      <td>Africa</td>
      <td>1,172,369.00</td>
      <td>79.49</td>
      <td>1.46</td>
    </tr>
    <tr>
      <td>Guyana</td>
      <td>South America</td>
      <td>790,329.00</td>
      <td>3.95</td>
      <td>1.46</td>
    </tr>
    <tr>
      <td>South Africa</td>
      <td>Africa</td>
      <td>60,041,996.00</td>
      <td>46.75</td>
      <td>1.41</td>
    </tr>
    <tr>
      <td>Dominican Republic</td>
      <td>North America</td>
      <td>10,953,714.00</td>
      <td>222.87</td>
      <td>1.38</td>
    </tr>
  </tbody>
</table>



    
![png](../projeto_covid_files_april_2022/projeto_covid_108_0.png)
    

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/47.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, passe o mouse pelos círculos ou clique nos nomes dos continentes para mostrá-los ou não. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>

Vemos que as 7 maiores densidades populacionais do mundo estão com R abaixo de 1. Ao menos pelo gráfico, não parece haver uma correlação tão direta entre as duas variáveis, mas como são dinâmicas o ideal é fazer uma análise temporal e não estática como no caso. Perceba, também, que a variável escolhida para o tamanho dos círculos foi o total de casos por milhão em cada localidade. Veja que há localidades com grande quantidade de casos acumulados mas com R < 1, indicando que já tiveram momentos de maior contágio no passado mas que, em abril de 2022, estavam em momentos de retração de contágio.

## Evolução do número de mortes

<center><img alt="covid_banner" width="50%" src="https://image.freepik.com/free-vector/coronavirus-cells-banner_1035-18749.jpg"></center>

Infelizmente alguns casos acabam levando a mortes. Vamos analisar como foi a evolução dos números.

### Total de mortes

#### Mundo 

Começando pelo total de mortes no mundo:


    
![png](../projeto_covid_files_april_2022/projeto_covid_113_0.png)
    


Observe que a ordem de grandeza dos eixos verticais é bem menor que a vista no gráfico análogo referente ao número de casos. Voltaremos a esse ponto posteriormente.

Vejamos se há alguma possível relação entre o total de óbitos e as variantes:


    
![png](../projeto_covid_files_april_2022/projeto_covid_115_0.png)
    


Enquanto nos gráficos de casos era possível perceber aumentos e eventualmente relacioná-los com variantes, vemos nos gráficos de mortes que não é tão evidente tal relação. Mesmo no caso da variante ômicron, não vemos um aumento na taxa de óbitos. Isto é um indicativo de que as variantes são menos letais que o vírus original. Na [página do CDC - Centers for Disease Control and Prevention](https://www.cdc.gov/coronavirus/2019-ncov/variants/omicron-variant.html) americano, o órgão afirma que a ômicron se espalha mais que as variantes anteriores, mesmo pessoas vacinadas podem contrair e passar a variante, mas é menos severa que as variantes anteriores.

#### Principais países

##### Totais absolutos

Vamos verificar os países que possuem os maiores totais de óbitos até abril de 2022:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>continent</th>
      <th>population</th>
      <th>total_deaths</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>United States</td>
      <td>North America</td>
      <td>332,915,074</td>
      <td>993,665</td>
    </tr>
    <tr>
      <td>Brazil</td>
      <td>South America</td>
      <td>213,993,441</td>
      <td>663,736</td>
    </tr>
    <tr>
      <td>India</td>
      <td>Asia</td>
      <td>1,393,409,033</td>
      <td>523,843</td>
    </tr>
    <tr>
      <td>Russia</td>
      <td>Europe</td>
      <td>145,912,022</td>
      <td>368,319</td>
    </tr>
    <tr>
      <td>Mexico</td>
      <td>North America</td>
      <td>130,262,220</td>
      <td>324,334</td>
    </tr>
    <tr>
      <td>Peru</td>
      <td>South America</td>
      <td>33,359,415</td>
      <td>212,810</td>
    </tr>
    <tr>
      <td>United Kingdom</td>
      <td>Europe</td>
      <td>68,207,114</td>
      <td>175,081</td>
    </tr>
    <tr>
      <td>Italy</td>
      <td>Europe</td>
      <td>60,367,471</td>
      <td>163,507</td>
    </tr>
    <tr>
      <td>Indonesia</td>
      <td>Asia</td>
      <td>276,361,788</td>
      <td>156,257</td>
    </tr>
    <tr>
      <td>France</td>
      <td>Europe</td>
      <td>67,422,000</td>
      <td>145,999</td>
    </tr>
  </tbody>
</table>



    
![png](../projeto_covid_files_april_2022/projeto_covid_118_0.png)
    


Vemos que o Brasil aparece em segundo, atrás apenas dos Estados Unidos. Também observamos a presença de outro país da América do Sul entre os 10 países: o Peru. Como tal país não possui população tão numerosa quanto os primeiros colocados, é um sinal que a proporção de mortes por milhão é alta, o que veremos mais adiante.

Vamos verificar o total de óbitos até o momento por país de cada continente:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/49.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique nos continentes para ter mais 
detalhes do mesmo. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>


##### Totais por milhão de habitantes

Como já discutido ao apresentar os dados referentes a casos de COVID-19, números absolutos por vezes distorcem nossa percepção sobre os impactos reais em cada localidade. Faz mais sentido avaliar os números por milhão de habitantes. Vamos, então, construir a tabela e o gráfico:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>continent</th>
      <th>total_deaths_per_million</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Peru</td>
      <td>South America</td>
      <td>6,379.31</td>
    </tr>
    <tr>
      <td>Bulgaria</td>
      <td>Europe</td>
      <td>5,352.31</td>
    </tr>
    <tr>
      <td>Bosnia and Herzegovina</td>
      <td>Europe</td>
      <td>4,830.76</td>
    </tr>
    <tr>
      <td>Hungary</td>
      <td>Europe</td>
      <td>4,795.54</td>
    </tr>
    <tr>
      <td>North Macedonia</td>
      <td>Europe</td>
      <td>4,455.84</td>
    </tr>
    <tr>
      <td>Montenegro</td>
      <td>Europe</td>
      <td>4,321.31</td>
    </tr>
    <tr>
      <td>Georgia</td>
      <td>Asia</td>
      <td>4,223.10</td>
    </tr>
    <tr>
      <td>Croatia</td>
      <td>Europe</td>
      <td>3,877.84</td>
    </tr>
    <tr>
      <td>Czechia</td>
      <td>Europe</td>
      <td>3,744.21</td>
    </tr>
    <tr>
      <td>Slovakia</td>
      <td>Europe</td>
      <td>3,648.01</td>
    </tr>
  </tbody>
</table>



    
![png](../projeto_covid_files_april_2022/projeto_covid_123_0.png)
    


Percebe-se claramente que a situação do Peru é diferenciada. E tal situação vem chamando a atenção [da mídia mundial](https://www.bbc.com/news/world-latin-america-53150808) e de [periódicos especializados](https://www.bmj.com/content/373/bmj.n1442). As condições precárias em regiões mais pobres do país e problemas no setor de saúde parecem  contribuir para o alto número de óbitos. Da mesma forma, a situação do leste europeu [tem gerado manchetes](https://www.ft.com/content/06b30dfb-998e-443f-a2bd-41f0b2ca4ab9) nos principais veículos de mídia mundiais.

Vamos analisar a situação de cada país por continente:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/51.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique nos continentes para ter mais 
detalhes do mesmo. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>


Observe como a Dinamarca, e seu território Ilhas Faroe, líder em casos por milhão, aparece como um dos países da Europa com menos mortes por milhão. Como discutido em outros trechos deste estudo, a mortalidade pode estar mais relacionada com aspectos sócio-econômicos e com o sistema de saúde de cada localidade do que com a agressividade da doença em si.

Outro país que merece destaque é a Suécia. Na contramão de outros países europeus, em especial seus vizinhos da Escandinávia, o país não adotou políticas restritivas como lockdown durante a pandemia. Devido ao [sistema legal do país](https://en.wikipedia.org/wiki/Swedish_government_response_to_the_COVID-19_pandemic), as decisões costumam ser descentralizadas para os ministérios, e estes não têm poder de lei. O Ministério da Saúde local adotou uma política de conscientização da população. As fronteiras com outros países foram fechadas, mas internamente as medidas foram brandas. Vemos nos dados de casos e de mortes que o país está dentre os menores números da Europa, [chamando a atenção da mídia](https://www.telegraph.co.uk/global-health/science-and-disease/swedens-death-rate-among-lowest-europe-despite-avoiding-strict/), indicando que [tal política não levou ao caos](https://www.bmj.com/content/375/bmj.n3081) que apoiadores de políticas restritivas diziam no início da pandemia. No entanto, como vemos nos gráficos, os números da Suécia são maiores que seus vizinhos escandinavos. [Alguns estudos](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7797349/) mostram que ocorreram mais mortes na porção de mais alta idade (acima de 80 anos) no país, comparado aos seus vizinhos, indicando possíveis problemas em casas de cuidados para idosos. Internamente, a visão é de que [a política não restritiva foi, em geral, correta](https://www.cbc.ca/news/world/sweden-report-coronavirus-1.6364154).

Já fiz uma pequena discussão na parte de casos acerca de países que provavelmente estão com dados abaixo dos números reais. Mas aqui cabe mais algumas observações sobre este tópico.

Começando pela América do Sul, percebemos a quase ausência da Venezuela, com um número muito abaixo de seus vizinhos. Obviamente que a situação chamou a atenção a ponto de a [Nature](https://www.nature.com/articles/d41586-021-02276-1) e o periódico [BMJ](https://www.bmj.com/content/371/bmj.m3938) irem verificar a situação. Como é de se esperar de regimes ditatoriais, há grande opressão governamental para abafar os casos e óbitos, além de falta de recursos básicos para controle da situação, conforme descrito nos links.

Na América do Norte, apesar de boa parte da atenção midiática ser destinada aos Estados Unidos, vemos que Trinidade e Tobago e México possuem números de óbitos por milhão de habitantes comparáveis aos americanos.

Por fim, um pouco sobre a África e a Ásia. De acordo com as [Nações Unidas](https://unstats.un.org/unsd/demographic-social/crvs/#coverage) parte significativa dos países africanos e asiáticos não possuem registros de mortes confiáveis, conforme figura abaixo retirada do site da instituição. Desta forma, cuidado deve ser tomado ao se fazer comparações com estes países.

<center><img alt="covid_banner" width="60%" src="https://unstats.un.org/unsd/demographic-social/crvs/documents/DeathCov.jpg"></center>

Há também [notícias como esta](https://www.bbc.com/news/world-africa-55674139) a respeito dos problemas de registro de óbitos em países africanos.

Vejamos um mapa interativo da evolução das mortes por milhão em cada país:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/53.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique na seta de play. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>



#### Idade e riqueza

Vamos verificar se o número de mortes possui alguma relação com a renda per capita do país ou a idade da população.

Comecemos verificando os países com maior PIB per capita:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>continent</th>
      <th>total_deaths</th>
      <th>total_deaths_per_million</th>
      <th>gdp_per_capita</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Qatar</td>
      <td>Asia</td>
      <td>677.00</td>
      <td>231.02</td>
      <td>116,935.60</td>
    </tr>
    <tr>
      <td>Macao</td>
      <td>Asia</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>104,861.85</td>
    </tr>
    <tr>
      <td>Luxembourg</td>
      <td>Europe</td>
      <td>1,064.00</td>
      <td>1,676.08</td>
      <td>94,277.96</td>
    </tr>
    <tr>
      <td>Singapore</td>
      <td>Asia</td>
      <td>1,335.00</td>
      <td>244.79</td>
      <td>85,535.38</td>
    </tr>
    <tr>
      <td>Brunei</td>
      <td>Asia</td>
      <td>218.00</td>
      <td>493.74</td>
      <td>71,809.25</td>
    </tr>
    <tr>
      <td>Ireland</td>
      <td>Europe</td>
      <td>7,087.00</td>
      <td>1,422.26</td>
      <td>67,335.29</td>
    </tr>
    <tr>
      <td>United Arab Emirates</td>
      <td>Asia</td>
      <td>2,302.00</td>
      <td>230.41</td>
      <td>67,293.48</td>
    </tr>
    <tr>
      <td>Kuwait</td>
      <td>Asia</td>
      <td>2,555.00</td>
      <td>590.27</td>
      <td>65,530.54</td>
    </tr>
    <tr>
      <td>Norway</td>
      <td>Europe</td>
      <td>2,932.00</td>
      <td>536.44</td>
      <td>64,800.06</td>
    </tr>
    <tr>
      <td>Switzerland</td>
      <td>Europe</td>
      <td>13,712.00</td>
      <td>1,573.29</td>
      <td>57,410.17</td>
    </tr>
  </tbody>
</table>


Já foi mostrado que a base de dados possui valores para agregados de países de acordo com a classificação de renda do Banco Mundial. Vejamos como estes agregados se comportam com relação ao total de mortes por milhão:


    
![png](../projeto_covid_files_april_2022/projeto_covid_131_0.png)
    


Vemos claramente que há mais mortes em países de renda alta, com quase duas ordens de grandeza mais casos que países de renda baixa. O perfil de crescimento é similar em todas as classificações. Observe como a faixa de valores deste gráfico, com máximo na ordem de 10<sup>3</sup>, é bem mais baixa que a do gráfico análogo visto na seção sobre casos, cujo máximo era na ordem de 10<sup>5</sup>.

Países com maiores rendas tendem a ter uma população mais velha. Vejamos como isto se relaciona com mortes por COVID-19:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/55.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, passe o mouse pelos círculos ou clique nos nomes dos continentes para mostrá-los ou não. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>

No gráfico, vemos claramente que os países do continente africano possuem, em sua maioria, menores expectativas de vida e menores PIB per capita (menores círculos no gráfico). Seicheles, que é país africano com maior PIB per capita com uma das maiores expectativas de vida do continente e [chegou a liderar os dados mundiais em total de casos por milhão](https://franciscobustamante.com.br/portfolio/2021-10-projeto_covid/#total-de-casos) ao final de 2021, fica em segundo no continente no que diz respeito a mortes por milhão. No entanto, países dos demais continentes possuem números maiores de mortes.

Vemos uma tendência de localidades com maiores expectativas de vida apresentarem mais óbitos. Obviamente que países mais ricos tendem a ter maior parcela da população com idade avançada e, como a doença afeta mais pessoas idosas, é de se esperar maior quantidade de óbitos nesses países. No entanto, cabe destacar que há notáveis exceções nessa tendência como, por exemplo, o Japão, sobre o qual já fizemos uma extensa análise anteriormente. Singapura e Coreia do Sul também são notáveis exceções, sendo que vimos anteriormente que a Coreia do Sul figura entre os países com maiores números de casos totais. Vemos, portanto, que mais casos não necessariamente levam a mais óbitos. A Coreia do Sul [não adotou lockdown](https://en.wikipedia.org/wiki/COVID-19_pandemic_in_South_Korea) como política de controle nem fechou estabelecimentos comerciais. Foi adotava uma política de muita testagem e de controle de distanciamento via celular e uso de cartão de crédito. O que gerou [desconforto na população](https://au.news.yahoo.com/coronavirus-thousands-defy-rally-ban-south-korea-cases-spike-215614353.html). E a política de hospitalização focou em pacientes de alto risco.

No gráfico, vemos que a expectativa de vida peruana é maior que a do Brasil, por exemplo, mas que o total de mortes por milhão de habitantes por COVID-19 é mais que o dobro da brasileira. Assim como já discutido em outros momentos do estudo, não se pode tirar grandes conclusões de apenas uma variável.

Uma maior expectativa de vida, no entanto, não necessariamente significa que a população atual possui mais idosos que jovens. Assim, vamos repetir o gráfico substituindo o eixo de expectativa de vida por mediana de idade:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/57.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, passe o mouse pelos círculos ou clique nos nomes dos continentes para mostrá-los ou não. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>

Vemos a mesma tendência: população mais idosa, mais mortes. No entanto, o caso do Peru se torna ainda mais chamativo por fugir da tendência dos demais países. 

O segundo país com mais mortes por milhão é a Bulgária. Se ampliarmos o gráfico anterior na porção de maiores medianas de idade, veremos que a Bulgária possui a oitava maior mediana de idade do mundo (44,7 anos), mas é o país com menor PIB per capita entre os 10 países com maior mediana de idade. O país [passa por uma crise demográfica](https://en.wikipedia.org/wiki/Bulgaria#Demographics) há alguns anos, com sua população envelhecendo e diminuindo de tamanho, além de ter uma parcela cada vez maior da população próximo ao nível de pobreza. Este retrato búlgaro é similar, guardadas as especificidades de cada país, aos demais países do leste europeu que aparecem na listagem de mais mortes por milhão. A [Bulgária acabou com restrições relativas ao COVID-19](https://www.schengenvisainfo.com/news/bulgaria-officially-lifts-all-covid-19-restrictions-on-may-1/) no início de maio.

O Peru possui uma mediana de idade bem menor que a búlgara, 29,1 anos, e um PIB per capita um terço menor. Conforme destacado em [notícias](https://www.bbc.com/news/world-latin-america-53150808) e [artigos](https://www.bmj.com/content/373/bmj.n1442), a situação de fragilidade econômica da população pode ter colaborado significativamente para a situação. Nas referências linkadas vemos, por exemplo, que cerca de 40 % das residências peruanas não possuem geladeiras, e estimasse que mais de 10 % das residências possuem mais pessoas que o recomendável para seus tamanhos. O país foi um dos primeiros a decretar lockdown na América do Sul, sendo até considerado modelo neste aspecto embora claramente [não tenha ajudado](https://onlinelibrary.wiley.com/doi/10.1111/eci.13484), e a fornecer auxílio financeiro à sua população. No entanto, mais de um terço da população adulta não possui contas bancárias, não tendo acesso fácil ao auxílio.

Outra comparação de interesse entre os dois países é quanto à vacinação de suas respectivas populações. Os dados completos de vacinação serão apresentados adiante, mas já podemos adiantar que o Peru possui 80 % de sua população com esquema vacinal completo, enquanto que a Bulgária possui apenas 29 %. Desta forma, não parece ser razoável explicar a situação peruana com base em apenas vacinas.

### Novas mortes

Até o momento focamos em avaliar a evolução do total de óbitos no mundo e em cada país. Agora, vamos verificar a evolução de novos óbitos, à medida que foram sendo reportados. Assim, podemos verificar em quais momentos ocorreram as popularmente chamadas "ondas".

A base de dados apresenta as entradas na forma de dados diários e na forma de média móvel de 7 dias. A média móvel ajuda a visualizar melhor a tendência dos dados, visto que há grande variação dos dados diários. Vamos começar com as mortes em todo mundo:


    
![png](../projeto_covid_files_april_2022/projeto_covid_138_0.png)
    


Vemos que os máximos em novos óbitos acompanham os máximos de novos casos já discutido anteriormente. Nota-se, entretanto, uma grande diminuição na ordem de grandeza de casos para óbitos, além de verificarmos uma tendência de queda nos óbitos. Por exemplo, enquanto a variante ômicron levou a um grande aumento de casos, vemos que, em termos de óbitos, não houve um aumento tão grande quando comparamos com outras variantes. Vejamos nos próximos gráficos, com as datas de variantes e épocas de inverno:


    
![png](../projeto_covid_files_april_2022/projeto_covid_140_0.png)
    



    
![png](../projeto_covid_files_april_2022/projeto_covid_141_0.png)
    


A sazonalidade pode ser observada nos mapas interativos de novas mortes e novas mortes por milhão:

<!-- TODO verificar numeração novas mortes por milhão  -->
<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/59.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique na seta de play. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/53.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique na seta de play. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>


Vejamos como foi o comportamento de novas mortes considerando agregados de países com base na classificação de renda do Banco Mundial:


    
![png](../projeto_covid_files_april_2022/projeto_covid_146_0.png)
    


Assim como já havíamos discutido para novos casos, vemos que as ondas possuem maior impacto em países de mais alta renda. A variante ômicron, na passagem de 2021 para 2022, é perceptível em todas as classificações. No entanto, vemos que o incremento de mortes da ômicron é menor que de outras ondas do passado. Isto corrobora o que vem sendo mostrado no estudo e nas referências, que a ômicron é menos letal que variantes antecedentes.

## Evolução no número de vacinados

<center><img alt="covid_banner" width="50%" src="https://image.freepik.com/free-photo/close-up-hand-holding-coronavirus-vaccine_23-2149012368.jpg"></center>

Certamente um dos pontos mais discutidos recentemente no contexto da pandemia. Vamos verificar a evolução.

### Total de vacinados

Começando pelo total de vacinados:

    
![png](../projeto_covid_files_april_2022/projeto_covid_151_0.png)
    


Os gráficos começam em dezembro de 2020, quando as primeiras doses foram aplicadas ao redor do mundo. Vemos que, até abril de 2022, cerca de 5,1 bilhões de pessoas já receberam ao menos uma dose da vacina, praticamente dois terços da população mundial. Os saltos nos gráficos se devem a questões de disponibilização dos dados.

Vejamos quantas pessoas já receberam todas as doses protocolares:


    
![png](../projeto_covid_files_april_2022/projeto_covid_153_0.png)
    


Vemos que uma parcela significativa da população mundial já tomou todas as doses protocolares. [Segundo as diretrizes da OWID](https://github.com/owid/covid-19-data/tree/master/public/data/vaccinations), são consideradas totalmente vacinadas as pessoas que receberam todas as doses inicialmente previstas no protocolo de cada vacina. Por exemplo, se uma vacina era prevista como sendo de duas doses, pessoas com duas doses são consideradas completamente vacinadas, mesmo que posteriormente não tenham tomado as doses de reforço. As doses de reforço são consideradas a parte, e podemos avaliá-las também:


    
![png](../projeto_covid_files_april_2022/projeto_covid_155_0.png)
    


Vemos que o número de doses de reforço aplicadas vem subindo consideravelmente desde o meio de 2021.

Vejamos quais países possuem mais pessoas vacinadas com ao menos uma dose:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>continent</th>
      <th>population</th>
      <th>people_vaccinated</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>China</td>
      <td>Asia</td>
      <td>1,444,216,102</td>
      <td>1,284,935,000</td>
    </tr>
    <tr>
      <td>India</td>
      <td>Asia</td>
      <td>1,393,409,033</td>
      <td>1,003,162,202</td>
    </tr>
    <tr>
      <td>United States</td>
      <td>North America</td>
      <td>332,915,074</td>
      <td>257,327,049</td>
    </tr>
    <tr>
      <td>Indonesia</td>
      <td>Asia</td>
      <td>276,361,788</td>
      <td>199,346,528</td>
    </tr>
    <tr>
      <td>Brazil</td>
      <td>South America</td>
      <td>213,993,441</td>
      <td>182,599,066</td>
    </tr>
    <tr>
      <td>Pakistan</td>
      <td>Asia</td>
      <td>225,199,929</td>
      <td>134,313,917</td>
    </tr>
    <tr>
      <td>Bangladesh</td>
      <td>Asia</td>
      <td>166,303,494</td>
      <td>128,729,921</td>
    </tr>
    <tr>
      <td>Japan</td>
      <td>Asia</td>
      <td>126,050,796</td>
      <td>103,153,093</td>
    </tr>
    <tr>
      <td>Mexico</td>
      <td>North America</td>
      <td>130,262,220</td>
      <td>85,797,486</td>
    </tr>
    <tr>
      <td>Russia</td>
      <td>Europe</td>
      <td>145,912,022</td>
      <td>80,862,100</td>
    </tr>
  </tbody>
</table>



    
![png](../projeto_covid_files_april_2022/projeto_covid_161_0.png)
    


Vemos aqui que China e Índia, considerando suas imensas populações, já vacinaram com ao menos uma dose mais de três quartos das pessoas. Brasil e Estados Unidos também possuem quantidades significativas de pessoas vacinadas. Lembrando que não necessariamente o número a ser buscado é 100 % tendo em vista que os protocolos atuais não preveem vacinações em crianças.

Vamos verificar cada país por continente:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/63.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique nos continentes para ter mais 
detalhes do mesmo. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>


Observe que o gráfico acima possui menos países que os mostrados anteriormente para casos e óbitos. Isto se deve ao fato de que nem todos os países possuem registros oficiais de vacinados de onde a OWID possa obter dados. O intervalo de disponibilização dos dados também é distinto para cada país, de forma que comparações devem ser feitas com cautela.

Assim como para casos e óbitos, análises comparativas fazem mais sentido com números relativos ao total da população de cada localidade. Vejamos então os locais com maior percentual da população vacinada com ao menos uma dose:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>continent</th>
      <th>population</th>
      <th>people_vaccinated_per_hundred</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Gibraltar</td>
      <td>Europe</td>
      <td>33,691.00</td>
      <td>124.88</td>
    </tr>
    <tr>
      <td>United Arab Emirates</td>
      <td>Asia</td>
      <td>9,991,083.00</td>
      <td>98.99</td>
    </tr>
    <tr>
      <td>Portugal</td>
      <td>Europe</td>
      <td>10,167,923.00</td>
      <td>95.42</td>
    </tr>
    <tr>
      <td>Cuba</td>
      <td>North America</td>
      <td>11,317,498.00</td>
      <td>94.17</td>
    </tr>
    <tr>
      <td>Samoa</td>
      <td>Oceania</td>
      <td>200,144.00</td>
      <td>93.63</td>
    </tr>
    <tr>
      <td>Brunei</td>
      <td>Asia</td>
      <td>441,532.00</td>
      <td>93.32</td>
    </tr>
    <tr>
      <td>Chile</td>
      <td>South America</td>
      <td>19,212,362.00</td>
      <td>93.25</td>
    </tr>
    <tr>
      <td>Malta</td>
      <td>Europe</td>
      <td>516,100.00</td>
      <td>92.13</td>
    </tr>
    <tr>
      <td>Singapore</td>
      <td>Asia</td>
      <td>5,453,600.00</td>
      <td>91.91</td>
    </tr>
    <tr>
      <td>Cayman Islands</td>
      <td>North America</td>
      <td>66,498.00</td>
      <td>91.43</td>
    </tr>
  </tbody>
</table>



    
![png](../projeto_covid_files_april_2022/projeto_covid_166_0.png)
    


Obviamente que o valor para Gibraltar está equivocado. [De acordo com a OWID](https://ourworldindata.org/covid-vaccinations#frequently-asked-questions), valores maiores do que 100 % podem ocorrer quando os valores para população estão desatualizados e/ou quando pessoas não residentes nos locais acabam entrando na conta como, por exemplo, turistas. No caso de Gibraltar, o aspecto turístico pode estar causando essa distorção.

Como era de se esperar, vemos o predomínio de localidades relativamente pequenas no que diz respeito ao número de habitantes, aparecendo apenas o Chile com população acima de 15 milhões de pessoas. Vejamos um gráfico de países por continente:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/65.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique nos continentes para ter mais 
detalhes do mesmo. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>


Observa-se que, para ao menos uma dose, a cobertura vacinal está razoável, com cerca de dois terços da população mundial (considerando apenas os países para os quais há dados). Apenas o continente africano ainda possui a maior parte dos países com menos de 50 % das pessoas vacinadas com ao menos uma dose.

Vejamos agora os números para pessoas completamente vacinadas:


<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>location</th>
      <th>continent</th>
      <th>population</th>
      <th>people_fully_vaccinated_per_hundred</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Gibraltar</td>
      <td>Europe</td>
      <td>33,691.00</td>
      <td>122.94</td>
    </tr>
    <tr>
      <td>United Arab Emirates</td>
      <td>Asia</td>
      <td>9,991,083.00</td>
      <td>96.42</td>
    </tr>
    <tr>
      <td>Brunei</td>
      <td>Asia</td>
      <td>441,532.00</td>
      <td>91.82</td>
    </tr>
    <tr>
      <td>Singapore</td>
      <td>Asia</td>
      <td>5,453,600.00</td>
      <td>91.25</td>
    </tr>
    <tr>
      <td>Chile</td>
      <td>South America</td>
      <td>19,212,362.00</td>
      <td>90.88</td>
    </tr>
    <tr>
      <td>Malta</td>
      <td>Europe</td>
      <td>516,100.00</td>
      <td>90.74</td>
    </tr>
    <tr>
      <td>Qatar</td>
      <td>Asia</td>
      <td>2,930,524.00</td>
      <td>88.80</td>
    </tr>
    <tr>
      <td>Cayman Islands</td>
      <td>North America</td>
      <td>66,498.00</td>
      <td>88.49</td>
    </tr>
    <tr>
      <td>Cuba</td>
      <td>North America</td>
      <td>11,317,498.00</td>
      <td>87.89</td>
    </tr>
    <tr>
      <td>Portugal</td>
      <td>Europe</td>
      <td>10,167,923.00</td>
      <td>86.93</td>
    </tr>
  </tbody>
</table>



    
![png](../projeto_covid_files_april_2022/projeto_covid_171_0.png)
    

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/67.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique nos continentes para ter mais 
detalhes do mesmo. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>


Alguns países merecem ser mencionados. Vimos no início deste estudo que a Dinamarca era o país com mais casos por milhão de habitantes e, agora, vemos que é o quinto mais vacinado da Europa. Também discutimos amplamente a situação de Austrália e Nova Zelândia, que enfrentam um grande aumento de casos da variante ômicron, sendo que ambos já vacinaram percentuais significativos de suas populações. Na América do Sul, o Peru, que já vacinou um percentual da população maior que o Brasil, por exemplo, possui o maior número óbitos por milhão de habitantes em todo o mundo. Na Ásia, mostramos que a Coreia do Sul é o oitavo país com mais casos no mundo e é o quinto mais vacinado na Ásia, sendo que possui números muito baixos de óbitos.

Assim, vemos que não é simples avaliar o impacto real das vacinações a partir de números tão genéricos. Citamos exemplos de países com elevadas taxas de vacinação que tiveram problemas com surtos de novos casos ou com elevado número de mortes. Cabe destacar que se está olhando um retrato, um momento no tempo. Não necessariamente um país com elevada taxa de vacinação agora começou a aplicação cedo, pode ser que tenha começado a aplicação tardiamente e eventuais benefícios das vacinas não tenham sido aproveitados por sua população, levando a casos mais graves e óbitos. Da mesma forma, comparando os casos de Peru e Bulgária com os de Japão e Coreia do Sul, todos detalhados durante este estudo, vemos que o preparo da rede de saúde e as condições de vida da população têm efeito imenso no resultado final de casos e óbitos, eventualmente até mais que a vacinação em si.

Vejamos se há alguma relação entre quantidade de vacinados e PIB per capita e se países com pessoas mais velhas estão se vacinando mais:


    
![png](../projeto_covid_files_april_2022/projeto_covid_175_0.png)
    


Neste primeiro gráfico, vemos como países de mais alta renda possuem um percentual de totalmente vacinados bem maior que baixas de renda mais baixa. Além disso, vemos como os classificados como de renda alta atingiram metade de pessoas vacinadas de forma muito mais rápida que países das demais classificações.

Podemos adicionar mais camadas de informação, avaliando o perfil de idade, por exemplo:

<iframe width="100%" height="500" frameborder="0" scrolling="no" src="//plotly.com/~chicolucio/69.embed?autosize=True"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, passe o mouse pelos círculos ou clique nos nomes dos continentes para mostrá-los ou não. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>

Como já discutido anteriormente, países com maior PIB per capita tendem a ter uma população mais velha e, pelo gráfico, uma maior parcela da população vacinada. Faz sentido se considerarmos que há custo envolvido em adquirir as vacinas, de forma que países mais ricos tendem a conseguir adquirir vacinas mais facilmente e a ter melhores condições de realizar grandes campanhas de vacinação. O dado para Luxemburgo não está atualizado [por problemas nos relatórios do país](https://github.com/owid/covid-19-data/issues/2153). É perceptível como países africanos, que possuem baixa mediana de idade e valores de PIB per capita baixos, também possuem baixo percentual de vacinados.

### Novas vacinações

Já vimos a evolução dos números totais, vejamos agora a evolução de novas vacinações:


    
![png](../projeto_covid_files_april_2022/projeto_covid_181_0.png)
    


Vemos que há uma tendência decrescente. Podemos considerar algumas hipóteses para este declínio que, provavelmente, ocorrem em conjunto:

- a mais evidente é que à medida que mais pessoas completam o protocolo de vacinação, menos vacinas são tomadas;
- países mais pobres estão tendo [dificuldades de adquirir vacinas](https://www.nature.com/articles/d41586-021-02383-z). Conforme o artigo do link, há pressão para que as patentes das tecnologias aplicadas pelas grandes farmacêuticas sejam quebradas para que empresas locais possam fabricar as vacinas a um menor custo. Ou que sejam feitas colaborações com as tais empresas;
- menos pessoas querendo se vacinar, por diversos motivos como (evidências nos links):
    - a crescente percepção pública e de cientistas de que as [vacinas são menos eficazes](https://www.nature.com/articles/d41586-021-02532-4) do que inicialmente se pensava, especialmente devido ao surgimento de variantes;
    - crescente desconfiança quanto a potenciais conflitos de interesse que eventualmente contribuíram para [a aprovação emergencial das vacinas](https://www.bmj.com/content/373/bmj.n1283) com [triagem diferenciada](https://ncirs.org.au/phases-clinical-trials) e para o boicote a medicamentos utilizados no que ficou conhecido como [tratamento precoce](https://www.sciencedirect.com/science/article/pii/S2052297520300627);
    - crescente revolta entre a população de diversos países frente à imposição dos chamados passaportes de vacinação. Exemplos [aqui](https://www.foxnews.com/world/protesters-pack-paris-streets-in-defiance-of-covid-19-vaccine-passport-our-freedoms-are-dying) e [aqui](https://www.bmj.com/content/375/bmj.n2575). Muito embora durante todo o estudo tenham sido citadas referências mostrando que boa parte dos países já retiraram total ou parcialmente as restrições referentes à COVID, é pouco provável que pessoas que se sentiram agredidas em suas liberdades durante o período repressivo busquem se vacinar agora que as restrições foram levantadas.
    - no mesmo sentido, a [Áustria, que implementou lockdown para não vacinados](https://youtu.be/y9io1MZz_7E), não vê seu número de vacinados aumentar, provavelmente porque tais pessoas que tiveram restringidas suas liberdades não se sentem motivadas a seguir recomendações forçadas. Além do aspecto de divisão social mostrado na referência. Desta forma, a crescente segregação das atividades permitidas ou não para aqueles que não se vacinaram pode estar contribuindo para que aqueles que não se vacinaram continuem com este posicionamento.
    - eventuais [riscos não previstos de algumas vacinas](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4072489).
    
Qual dos motivos apontados contribui mais ou se há mais algum motivo é difícil de avaliar com base nos dados disponíveis. No entanto, se verificarmos o perfil de novas vacinações com base na classificação de renda do Banco Mundial, gráfico a seguir, vemos que países de mais alta renda já estão com perfil decrescente de novas vacinações, enquanto que países de renda baixa estão em perfil crescente. Isto corrobora o escrito acima, sobre a dificuldade que países pobres estão tendo para adquirir vacinas.


    
![png](../projeto_covid_files_april_2022/projeto_covid_183_0.png)
    


## Comparações e correlações

<center><img alt="covid_banner" width="50%" src="https://image.freepik.com/free-photo/close-up-pen-market-research_1098-3465.jpg"></center>

Já analisamos casos, óbitos e vacinações de forma separada. Vamos começar a buscar comparações e correlações. Primeiro, vamos comparar a ordem de grandeza entre casos, mortes e vacinações nos continentes:


    
![png](../projeto_covid_files_april_2022/projeto_covid_186_0.png)
    


Como já abordado brevemente anteriormente, há uma diferença de cerca de duas ordens de grandeza entre casos e mortes. Podemos ver isto também em um gráfico com os mesmos dados por milhão de habitantes:


    
![png](../projeto_covid_files_april_2022/projeto_covid_188_0.png)
    


Esta observação está coerente com estudos que indicam que a mortalidade da COVID-19 fica em torno de 1 a 2 %. Cabe ressaltar que há diferentes formas de cálculo e interpretações para mortalidade [como mostra esse material da OMS](https://www.who.int/news-room/commentaries/detail/estimating-mortality-from-covid-19), especialmente com a pandemia em andamento. E já vimos que há diversas questões envolvendo a disponibilização de dados por parte de alguns países. 

No entanto, se confirmada essa faixa de mortalidade, a pandemia de COVID-19 seria [proporcionalmente menos fatal](https://www.news-medical.net/health/How-does-the-COVID-19-Pandemic-Compare-to-Other-Pandemics.aspx) que outras pandemias no último século. O que diferencia a COVID-19, segundo [estudos](https://tdtmvjournal.biomedcentral.com/articles/10.1186/s40794-020-00129-9), é sua alta transmissibilidade que a espalhou por todo o planeta, de forma que a contagem total de mortos se tornou elevada. Cabe ressaltar que, como toda comparação, cuidados devem ser tomados e há [estudos](https://www.cambridge.org/core/journals/infection-control-and-hospital-epidemiology/article/why-comparing-coronavirus-disease-2019-covid19-and-seasonal-influenza-fatality-rates-is-like-comparing-apples-to-pears/FDFE92B10895C57C42F59C2422DB7582) que abordam tais cuidados, que fogem ao escopo deste trabalho.

Durante a análise, em alguns momentos correlações de casos e óbitos com idade da população e PIB per capita foram estudadas. Vamos avaliar, através de [matrizes de correlação](https://en.wikipedia.org/wiki/Correlation), se há correlações positivas ou negativas entre variáveis. Comecemos procurando relações entre totais de casos/óbitos com vacinação, indicadores de riqueza e indicadores de idade:


    
![png](../projeto_covid_files_april_2022/projeto_covid_191_1.png)
    


Podemos criar matriz similar mas para novos casos/óbitos/vacinações e suas relações com índices que variam no tempo:


    
![png](../projeto_covid_files_april_2022/projeto_covid_193_1.png)
    


Nas figuras acima temos algumas correlações mais óbvias e que já foram discutidas no decorrer do estudo, que são:

- correlação positiva entre idades mais avançadas e maior PIB per capita;
- correlação positiva entre mais pessoas vacinadas e maior PIB per capita;
- como consequência das anteriores, correlação positiva entre pessoas vacinadas e idades mais avançadas

Outras correlações:

- correlação positiva entre pessoas vacinadas e casos por milhão;
- correlação negativa entre pessoas vacinadas e mortes por milhão;
- correlação positiva entre o índice de restrição e a taxa de reprodução efetiva;
- correlação positiva entre o índice de restrição e novas mortes;

Estas duas últimas correlações, embora fracas, encontram respaldo em algumas publicações científicas recentes, como [esta](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3711686) e [esta](https://onlinelibrary.wiley.com/doi/10.1111/eci.13484), que parecem indicar ineficácia ou mesmo piora no número de casos e óbitos devido ao uso de medidas restritivas.

Muito se fala sobre o efeito de comorbidades, vamos verificar se há correlações envolvendo as variáveis referentes a doenças:


    
![png](../projeto_covid_files_april_2022/projeto_covid_195_1.png)
    


Curiosamente há relação negativa entre taxa de mortalidade por doenças cardiovasculares (lembrando que os dados de comorbidades são de 2017) e casos e óbitos. E há relação positiva entre mulheres fumantes e casos e óbitos, assim como para homens mas com valores menores. Não se pode fazer análises muito profundas com base nestes dados pois não significa que as pessoas que tiveram os casos ou morreram tinham as doenças ou fumavam. Os dados de fumantes e doenças são para cada localidade e não para cada indivíduo.

## Brasil

<center><img alt="covid_banner" width="50%" src="https://image.freepik.com/free-photo/realistic-shot-waving-flag-brazil-with-interesting-textures_181624-11214.jpg"></center>

Para terminar, vamos abordar um pouco da situação brasileira. Em alguns momentos durante o estudo comentários já foram feitos acerca do país, então apenas complementaremos com mais algumas informações.

Comecemos verificando como a evolução de novos casos se relaciona com as datas de surgimento de novas variantes e com os períodos de inverno:


    
![png](../projeto_covid_files_april_2022/projeto_covid_199_0.png)
    


No gráfico há um aumento significativo de casos logo após a variante gama, cujo primeira amostra foi no país. No entanto, cabe ressaltar que as variantes podem demorar um certo tempo para se espalhar pelo mundo, de forma que as datas das outras variantes não necessariamente são estas no país.

Há picos de casos nos invernos, porém os maiores picos foram entre março e maio de 2021 e janeiro e fevereiro de 2022. E no último inverno tivemos um pico no início da estação mas um perfil decrescente na parte final.

O Ministério da Saúde, ao [fim de abril de 2022](https://www1.folha.uol.com.br/cotidiano/2022/04/queiroga-assina-portaria-que-acaba-com-a-emergencia-sanitaria-pela-covid.shtml), declarou o fim da emergência sanitária.

Vejamos o perfil de novos óbitos:


    
![png](../projeto_covid_files_april_2022/projeto_covid_201_0.png)
    


Vemos que o perfil de novos óbitos segue o de novos casos, mas em menor ordem de grandeza. Também é possível observar como a variante ômicron, que aumentou significativamente o número de casos, não aumentou na mesma proporção o número de óbitos.

É interessante observar como o Brasil se compara com os demais países da América do Sul e, também, com os demais países classificados como de renda média alta na classificação do Banco Mundial. Comecemos com o total de casos por milhão de habitantes:


    
![png](../projeto_covid_files_april_2022/projeto_covid_204_0.png)
    


Vemos que o Brasil acompanha o perfil da América do Sul. No entanto, possui valores acima do agregado de países de renda média alta. No [link do Banco Mundial](https://datahelpdesk.worldbank.org/knowledgebase/articles/906519-world-bank-country-and-lending-groups) sobre a classificação, vemos que nessa categoria há países como China, alguns africanos e asiáticos. Conforme descrito no início deste estudo, nem sempre números vindos destes locais são confiáveis. Desta forma, não necessariamente o país, e a América do Sul, estão tendo um desempenho pior para lidar com a doença que os demais países de mesma classificação.

Quanto a novos casos, total de mortes e novas mortes, temos os seguintes gráficos:


    
![png](../projeto_covid_files_april_2022/projeto_covid_206_0.png)
    



    
![png](../projeto_covid_files_april_2022/projeto_covid_207_0.png)
    



    
![png](../projeto_covid_files_april_2022/projeto_covid_208_0.png)
    


A mesma discussão feita para o total de casos por milhão pode ser estendida para novos casos, total de mortes e novas mortes.

A seguir, vemos que tanto o Brasil acompanhou o perfil da América do Sul e dos demais países classificados como de renda média alta no quesito vacinação.


    
![png](../projeto_covid_files_april_2022/projeto_covid_210_0.png)
    



    
![png](../projeto_covid_files_april_2022/projeto_covid_211_0.png)
    


Infelizmente a base de dados é bem incompleta para o Brasil. Há diversos dados ausentes referentes a hospitalizações e testes. Em breve farei um estudo focado no país utilizando dados oficias governamentais. Acompanhe meu [perfil no LinkedIn](https://www.linkedin.com/in/flsbustamante/) para saber quando eu divulgar. 


## Conclusões


<center><img alt="covid_banner" width="50%" src="https://image.freepik.com/free-photo/digital-world-map-hologram-blue-background_1379-901.jpg"></center>

Foi um longo estudo, mas espero que tenha sido proveitoso e que tenha trazido novas visões a respeito da pandemia. Busquei colocar links para diversas notícias e estudos com o intuito de embasar afirmações e facilitar a compreensão por parte dos leitores. Vamos fazer uma breve recaptulação do que foi mostrado:

- evolução de casos e mortes e possíveis relações com variantes 
- sazonalidade dos casos e mortes e possível relação com períodos de inverno
- importância de avaliar os dados por milhão de habitantes ao invés dos totais
- correlações existentes entre diversas variáveis

Caso tenha dúvidas, comentários e/ou críticas construtivas me procure:

- [LinkedIn](https://www.linkedin.com/in/flsbustamante/)
- [GitHub](https://github.com/chicolucio)
- [Site](https://franciscobustamante.com.br/)
- [Telegram](https://t.me/chicolucio)

Veja [o repositório no GitHub](https://github.com/chicolucio/panorama-covid-mundo) para ter acesso ao código e à análise completa. Ou, se preferir, clique no ícone abaixo para abrir o trabalho completo no Google Colab:


[![Abra no Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/chicolucio/panorama-covid-mundo/blob/master/projeto_covid_colab.ipynb)

Conheça [meu portfolio](https://franciscobustamante.com.br/portfolio/).
