---
name: Panorama do COVID-19 no mundo
title: Panorama do COVID-19 no mundo
image: /portfolio/projeto_covid_files/covid_logo.jpg
position: 1
period: Outubro de 2021
toc: true
header:
  og_image: /portfolio/projeto_covid_files/covid_logo.jpg
description: |
  Estamos completando quase 2 anos desde o surgimento do vírus COVID-19. As
  medidas para controle pandêmico adotadas por governos em todo mundo  afetaram
  significativamente a rotina de todos e é de grande interesse acompanharmos a
  evolução da pandemia para saber seus reais efeitos e avaliar o retorno ao
  convívio normal.
---

Por: [Francisco Bustamante](https://franciscobustamante.com.br/)

---

![png](../projeto_covid_files/covid_banner.jpg)

A COVID-19 é uma doença infecciosa causada por um recém-descoberto coronavírus.

Estamos completando quase 2 anos desde o surgimento do vírus COVID-19. As
medidas para controle pandêmico adotadas por governos em todo mundo  afetaram
significativamente a rotina de todos e é de grande interesse acompanharmos a
evolução da pandemia para saber seus reais efeitos e avaliar o retorno ao
convívio normal.

<center><img alt="covid_banner" width="100%"
src="https://raw.githubusercontent.com/chicolucio/panorama-covid-mundo/master/images/evolucao_casos_banner.gif"></center>
<p style="font-size:60%;color:gray">Animações como esta serão usadas no texto.
Se quiser saber como fazer e ainda interagir com as animações, acompanhe até
o final</p>

Neste trabalho, uma análise exploratória dos dados fornecidos pela Our World in
Data (OWID) é feita, mostrando o avanço temporal de casos, óbitos e vacinação.
Estudos comparativos e busca de correlações são feitos com intuito de melhor
compreender os dados. Animações como a mostrada acima são mostradas e
interpretadas.

## Obtenção dos dados e bibliotecas utilizadas

<center><img alt="covid_banner" width="20%"
src="https://ourworldindata.org/uploads/2019/02/OurWorldinData-logo.png"></center>

Como já citado, os dados aqui utilizados são fornecidos pela *Our World in Data*
(OWID) [neste repositório](https://github.com/owid/covid-19-data), uma
organização que reúne pesquisadores de todo o mundo para agregar dados de
diversas fontes sobre os mais diversos problemas contemporâneos.

Obviamente que por ser uma doença recente cuidados devem ser tomados no que diz
respeito à interpretação dos dados e relações de causa e efeito. Como será
mostrado adiante, nem todos os países fornecem dados completos e há algumas
inconsistências nos dados a depender de suas origens. Além disso, com o
crescente conhecimento a respeito da doença, por vezes se faz necessário alterar
a forma de agregar os dados, causando algumas dificuldades de análise temporal.
Não podemos ignorar também o fato de que, infelizmente, a visão passada por
veículos de mídia e governantes para população pode variar e ser distorcida a
depender dos interesses momentâneos dos mesmos.

Assim, durante esse estudo, especial cuidado é destinado em apresentar os dados
e buscar interpretá-los de forma rigorosa deixando claros eventuais limitações,
conflitos e condições de contorno de cada análise. E, sempre lembrando,
correlações não implicam em causalidade.

No total, a base de dados possui 65 variáveis e 122.175 entradas. Há ausência
de muitos dados sobre hospitalização e realização de testes por nem todos os 
países terem informações oficiais, de forma que essas categorias foram deixadas
de fora desta análise.

O pacote [Pandas](https://pandas.pydata.org/) foi utilizado para trabalhar com
os dados e construir matrizes de correlações. Os pacotes
[Matplotlib](https://matplotlib.org/), [Seaborn](https://seaborn.pydata.org/) e
[Plotly](https://plotly.com/) foram utilizados para construir os gráficos. A
biblioteca `datetime`, presente em uma instalação padrão do Python, foi
utilizada para lidar com datas. 

A base de dados baixada apresenta entradas até 8 de outubro de 2021, quando foi
obtida. Porém, há alguns dados cuja periodicidade é diferenciada.
Desta forma, vamos considerar como última data para análise 30 de setembro de
2021 para termos apenas meses fechados, de janeiro de 2020 até setembro de 2021.

## Evolução do número de casos

<center><img alt="covid_banner" width="50%"
src="https://image.freepik.com/free-photo/3d-medical-grunge-background-with-abstract-coronavirus-cells_1048-11987.jpg"></center>

Vamos começar nossa análise avaliando a evolução no número total de casos em todo o mundo.

### Total de casos

Muito do que é exposto nas mídias foca na quantidade total de casos por país ou
mundialmente. Vamos avaliar o perfil de crescimento no número de casos mundial:

![png](../projeto_covid_files/projeto_covid_23_0.png)
    
Vemos que, desde o surgimento em meados de janeiro de 2020, foram necessários
cerca de 3 meses para atingir 1 milhão de casos e 1 ano para 100 milhões de
casos. Lembrando que esses são os casos efetivamente computados devido a
testagem ou procura das pessoas pelas redes de saúde. O número real é
potencialmente maior tendo em vista que a maioria dos casos é branda ou
assintomática, de forma que os indivíduos não procuram as redes de saúde. De
qualquer forma, considerando que a população mundial é estimada na ordem de 7,8
bilhões de pessoas, **o número de casos até o momento indica que cerca de 3 % da
população mundial já teve a doença**.

Muito se fala das variantes do vírus. A Organização Mundial da Saúde (OMS) em
[seu site](https://www.who.int/en/activities/tracking-SARS-CoV-2-variants/)
descreve os critérios para considerar uma determinada variante como sendo de
interesse ou preocupante. A OMS também esclarece que os nomes são dados um certo
tempo depois que as primeiras amostras de cada variante são analisadas, conforme
a tabela:


| Variante | Data da primeira amostra | Data do nome |
|:------|:--------|:--------|
| alfa | Setembro/2020 - Reino Unido | Dezembro de 2020  | 
| beta | Maio/2020 - África do Sul | Dezembro de 2020  | 
| gama | Novembro/2020 - Brasil | Janeiro de 2021 | 
| delta | Outubro/2020 - Índia | Abril de 2021 | 
| lambda | Dezembro/2020 - Peru | Junho de 2021 | 
| mu | Janeiro/2021 - Colômbia | Agosto de 2021 | 

Neste estudo, optou-se por utilizar a data da primeira amostra nos gráficos,
motivo pelo qual a beta aparece antes da alfa. Tal escolha se deve ao fato de a
variante já estar se espalhando desde a data de amostra independentemente de ter
recebido um nome. Vamos adicionar aos gráficos anteriores linhas indicando as
datas de aparecimento de cada variante:
    
![png](../projeto_covid_files/projeto_covid_26_0.png)
    
O número de casos teve um acelerado crescimento inicial antes do surgimento da
primeira variante (beta) e houve um aumento de 10 vezes entre as datas da
variante beta e da variante alfa, um intervalo de 4 meses. O crescimento se
torna, então, mais lento. Nos quatro meses entre as variantes alfa e mu o
número de casos cresceu 3 vezes.

Vamos agora olhar os países com mais casos até setembro de 2021. 
Em um primeiro momento, podemos supor que países com maiores populações
apresentem mais casos. Vamos verificar as maiores populações mundiais:

| País | População |
|----|----|
| China |1.444.216.102 |
| Índia|1.393.409.033 |
| Estados Unidos |332.915.074 |
| Indonésia|276.361.788 |
| Paquistão |225.199.929 |
| Brasil|213.993.441 |
| Nigéria|211.400.704 |
| Bangladesh|166.303.494 |
| Rússia|145.912.022 |
| México|130.262.220 |



Agora, vamos verificar os países com mais casos:
    
![png](../projeto_covid_files/projeto_covid_42_0.png)
    

Vemos que 4 países dentre os que apresentam maiores populações no mundo também
figuram entre os com mais casos: Estados Unidos, Índia, Brasil e Rússia. Há a
ausência da China, mas os dados de lá não são confiáveis tendo em vista o regime
ditatorial, sendo que o país até hoje [se nega a
fornecer](https://www.forbes.com/sites/jemimamcevoy/2021/07/15/who-chief-china-not-sharing-critical-data-in-covid-origins-probe/?sh=411fe3e47591)
dados mais completos sobre a própria origem do vírus.

Outra ausência que chama a atenção é a Indonésia. Novamente, devido a questões
políticas, não se pode confiar totalmente nos dados reportados por tal país,
visto que o [próprio presidente da
Indonésia](https://www.thejakartapost.com/news/2020/03/13/we-dont-want-people-to-panic-jokowi-says-on-lack-of-transparency-about-covid-cases.html)
afirmou em dado ponto que não estavam disponibilizando informações para "evitar
pânico" na população.

Vemos por esses dois casos que há diversos fatores sociais, econômicos e
políticos que podem afetar a análise.  Há países entre as maiores populações que
enfrentam questões políticas, econômicas e de saúde complicadas, como Paquistão,
Bangladesh e Nigéria, que podem afetar a capacidade de tais países em acompanhar
e reportar os casos adequadamente. Inclusive, de acordo com [dados e relatórios
das Nações
Unidas](https://unstats.un.org/unsd/demographic-social/crvs/#coverage), apenas
dois terços dos países do mundo possuem registros de pelo menos 90 % dos
nascimentos e óbitos e todos os países citados aparecem nos relatórios como
países com dificuldades de registro.

Neste estudo, vamos nos ater ao que é apresentado. Mas é importante notar tais
situações. 


No gráfico interativo a seguir temos a situação de cada país por continente:


<iframe width="100%" height="500" frameborder="0" scrolling="no"
src="//plotly.com/~chicolucio/13.embed?autosize=true"></iframe>

<p style="font-size:60%; color:gray">
Para interagir com o gráfico, clique nos continentes para ter mais 
detalhes do mesmo. No celular pode ser que o título fique cortado pelo tamanho
da tela, mas funciona igual. 
</p>

No gráfico, temos uma ideia da proporção do total de casos de cada país frente
aos demais de seu continente e do mundo.

Mas será que essa é a melhor métrica para avaliar comparativamente a situação de
cada país?

<!-- ###### Totais por milhão de habitantes TODO deixar como negrito? -->

Uma análise do total absoluto de casos não necessariamente reflete a realidade
de cada país. Afinal, como já dito, países com maiores populações tendem a ter
maior número de casos. No entanto, isso não significa que países com populações
pequenas não possam estar em situação pior. Afinal, podem, relativamente ao seu
número de habitantes, estar com elevado número de casos considerando suas redes
de saúde. E regiões com maiores densidades populacionais podem favorecer uma
maior taxa de transmissão (hipótese que será estudada mais adiante).

Vamos, então, verificar os países com mais casos por milhão de habitantes:
    
![png](../projeto_covid_files/projeto_covid_51_0.png)
    
Percebemos uma grande mudança, aparecendo pequenos países. Em primeiro temos
Seicheles, um país insular do continente africano, tendo um grande fluxo de
turistas. Já em [maio de 2021](https://www.bbc.com/portuguese/geral-57207566)
notícias surgiam mostrando o aumento de casos no país apesar de ter sido o mais
vacinado do mundo (proporcionalmente à sua população) na época.

Inclusive, vemos no gráfico outros países que atingiram rapidamente elevadas
taxas de vacinação e, mesmo assim, tiveram picos de novos casos como, por
exemplo,
[Israel](https://www.npr.org/sections/goatsandsoda/2021/08/20/1029628471/highly-vaccinated-israel-is-seeing-a-dramatic-surge-in-new-covid-cases-heres-why).
Veremos dados de vacinação mais adiante.

Vamos ver em um gráfico interativo mostrando cada país por continente:

<iframe width="100%" height="500" frameborder="0" scrolling="no"
src="//plotly.com/~chicolucio/15.embed?autosize=true"></iframe>

Veja como a análise proporcional muda significativamente o perfil do gráfico.
Todos os países que apareciam como líderes em totais absolutos não figuram entre
os que possuem mais casos por milhão de habitantes, com exceção dos Estados
Unidos. Na Europa, vemos uma maior participação de países do leste europeu que,
inclusive, [parece estar entrando em uma nova
onda](https://edition.cnn.com/2021/10/25/uk/europe-covid-second-pandemic-winter-intl-gbr/index.html).
Na América do Sul, vemos que o Brasil se encontra em terceiro, atrás de
Argentina e Uruguai, países que [foram considerados
modelos](https://www.nexojornal.com.br/expresso/2021/04/15/As-respostas-de-Argentina-e-Uruguai-aos-recordes-de-covid0)
em diversos estágios da pandemia com suas medidas restritivas e lockdowns.

Vamos ver um mapa interativo mostrando a evolução do total de casos por
milhão em cada país. A consolidação foi feita por mês apenas para deixar a
animação do mapa mais rápida, se fosse evolução diária seria uma animação bem
demorada.

[![gif](../projeto_covid_files/projeto_covid_total_casos_milhao.gif)](https://plotly.com/~chicolucio/11/)
<p style="font-size:60%;color:gray">
Caso queira interagir com o mapa, clique na animação que você será redirecionado
para uma plataforma onde pode verificar mais detalhes. Mas termine de ler o texto
primeiro ;-) </p>

Vemos no mapa que apenas alguns países da África apresentaram significativa
evolução no número de casos. Como já escrito anteriormente, provavelmente isso
está relacionado com as precárias condições de tais países em reportar
adequadamente casos e mesmo realizar testes. No entanto, como será mostrado
adiante, o perfil mais jovem da população também pode ter alguma relação, já que
a doença é mais branda em jovens, o que pode fazer com que menos pessoas busquem
tratamento, ficando de fora dos números oficiais. Há [alguns
estudos](https://www.sciencedirect.com/science/article/pii/S120197122032172X)
que sugerem tal explicação, além de outras.

Vemos na Oceania poucos casos na Austrália e na Nova Zelândia. Os dois países
adotaram políticas altamente restritivas e questionáveis buscando zerar novos
casos e se [veem forçados a
afrouxar](https://www.bloomberg.com/news/articles/2021-08-22/n-z-says-delta-raises-questions-about-its-covid-zero-approach)
tais políticas por não terem tido o efeito desejado e estarem quebrando suas
economias.

#### Idade e PIB per capita

Para verificar se o número de casos possui alguma relação com o PIB per
capita do país ou a idade da população, vamos criar um
gráfico interativo buscando avaliar como os casos variam com a expectativa de
vida e o PIB per capita:

<iframe width="100%" height="500" frameborder="0" scrolling="no"
src="//plotly.com/~chicolucio/17.embed?autosize=true"></iframe>

No gráfico vemos claramente que os países do continente africano possuem, em sua
maioria, menores expectativas de vida e menores PIB per capita (menores círculos
no gráfico). No entanto, Seicheles, país africano com maior PIB per capita e uma
das maiores expectativas de vida do continente, lidera os dados mundiais em
total de casos por milhão.

Vemos uma tendência similar em outros continentes, maiores expectativas de vida
e PIB per capita com mais casos. Obviamente que países mais ricos tendem a ter
populações mais velhas ou, melhor dizendo, maior parcela da população com idade
avançada e, como a doença afeta mais pessoas idosas, é de se esperar maior
notificação de casos nesses países. No entanto, cabe destacar que há notáveis
exceções à essa tendência como, por exemplo, o Japão.

Uma maior expectativa de vida, no entanto, não necessariamente significa que a
população atual possui mais idosos que jovens. Assim, vamos repetir o gráfico
substituindo o eixo de expectativa de vida por mediana de idade:

<iframe width="100%" height="500" frameborder="0" scrolling="no"
src="//plotly.com/~chicolucio/19.embed?autosize=true"></iframe>

Vemos a mesma tendência: população mais velha, mais casos notificados.

### Novos casos

Até o momento, focamos em avaliar a evolução do total de casos no mundo e em cada
país. Agora, vamos verificar a evolução de novos casos, à medida que foram sendo
reportados. Assim, podemos verificar em quais momentos houve maior notificação,
as popularmente chamadas "ondas" de casos.

A base de dados apresenta as entradas na forma de dados diários e na forma de
média móvel de 7 dias. A média móvel ajuda a visualizar melhor a tendência dos
dados, visto que há grande variação dos dados diários. Vamos começar com os
casos em todo mundo:

![png](../projeto_covid_files/projeto_covid_66_0.png)
    
Vemos que está surgindo um padrão de aceleração de casos e máximos. Em 2020
vimos três grandes saltos em novos casos, por volta de março/abril, por volta de
junho/julho e por volta de outubro/novembro. Em 2021, já vimos novamente saltos
em abril e julho, ficando a expectativa de novo crescimento de casos na parte
final do ano.

Podemos verificar se há algum indício de relação entre novos casos e as
variantes:

![png](../projeto_covid_files/projeto_covid_68_0.png)
    
Em 2020, vemos que o aumento de casos entre junho e julho ocorre após o
surgimento da variante beta. De forma similar, após o surgimento da variante
delta, ocorre o aumento significativo de casos entre outubro e novembro. Isso
corrobora as afirmações veiculadas frequentemente na mídia que tal variante,
assim como as que surgiram posteriormente, são mais transmissíveis.

Agora, sabemos que em estações mais frias usualmente há [mais casos de
resfriados e gripes](https://www.cdc.gov/flu/about/season/flu-season.htm). Será
que há alguma relação entre inverno e casos de COVID-19? Vejamos:

![png](../projeto_covid_files/projeto_covid_70_0.png)
    
Vemos que há uma possível relação, repare os períodos entre junho e setembro de
ambos os anos e o período entre dezembro de 2020 e março de 2021. No entanto, o
máximo entre abril e maio de 2021 não encaixa muito bem nessa explicação. 

Vamos comparar os dados consolidados mundialmente com os dados individuais dos
países que vimos anteriormente possuir mais casos:

![png](../projeto_covid_files/projeto_covid_72_0.png)
    
Aqui vemos mais claramente como o pico entre abril e maio de 2021 possui grande
influência da Índia e talvez seja decorrente de algo específico deste país. Na
época, [notícias](https://www.nature.com/articles/d41586-021-01059-y) também
demonstravam uma certa dúvida nos motivos que levaram a Índia a ter esse surto
de casos, suspeitando-se de novas variantes ou de espalhamento para os grandes
centros urbanos.

Vamos verificar o perfil de cada continente, ainda comparando com o acumulado
mundial:

![png](../projeto_covid_files/projeto_covid_75_0.png)
    
Aqui percebemos que o perfil da Ásia não acompanha fielmente o da Europa e o da
América do Norte, mesmo estando todos estes continentes no mesmo hemisfério.
Mas, como vimos, há grande efeito dos números da Índia no continente asiático e
há diversos países suspeitos de subnotificação no mesmo continente, de forma que
a comparação pode ser prejudicada.

Quanto as continentes do hemisfério sul, não se pode afirmar claramente se há
alguma tendência conjunta. 

A dificuldade em encontrar padrões entre continentes é resultado da grande
diferença de população entre os mesmos, além da posição geográfica e de
possíveis subnotificações já citadas. Não necessariamente pertencer ao mesmo
hemisfério significa ter estações do ano similares. Além de haver diversos
fatores relacionados ao microclima de cada país/região e, obviamente, aspectos
políticos e econômicos muito distintos. Apenas de forma ilustrativa, vejamos a
população de cada continente:

|Continente | População |
| ---- | ----- |
|  Oceania | 43.219.954 | 
|  América do Sul | 434.260.138 | 
|  América do Norte | 596.581.283 | 
|  Europa | 747.747.396 | 
|  África | 1.373.486.472 | 
|  Ásia | 4.679.660.580 | 

Já vimos que aparentemente há um caráter sazonal nos casos de COVID-19. Vamos
criar um mapa que apresente esse perfil para cada país.
Assim como já discutido para o total de casos, talvez seja mais elucidativo
comparar os novos casos por milhão de habitantes, para diminuir eventuais
efeitos de países com grandes populações:

[![gif](../projeto_covid_files/projeto_covid_novos_casos_milhao.gif)](https://plotly.com/~chicolucio/21/)
<p style="font-size:60%;color:gray">
Caso queira interagir com o mapa, clique na animação que você será redirecionado
para uma plataforma onde pode verificar mais detalhes. Mas termine de ler o texto
primeiro ;-)</p>

Observe como as cores oscilam para os países enquanto o tempo passa, e como as
oscilações não são sincronizadas. Compare, por exemplo, os Estados Unidos e o
Brasil para perceber essa falta de sincronia.

Agora, será que há alguma forma de medir a velocidade de transmissão da doença?

### Taxa efetiva de reprodução (R)

A taxa efetiva de reprodução (R) é o número médio de pessoas infectadas em
determinado momento por um indivíduo infectado introduzido em uma população
*parcialmente imune*. Atenção ao "parcialmente imune", pois no início da
pandemia muito se falava do R<sub>0</sub>, que é um conceito análogo mas para
uma população completamente suscetível, ou seja, sem indivíduos imunizados seja
por contágio prévio ou por vacinação.

A
[interpretação](https://www.dw.com/pt-br/o-que-%C3%A9-o-n%C3%BAmero-de-reprodu%C3%A7%C3%A3o-r/a-53397119)
do valor calculado de R é simples:

- R > 1: o número de casos da doença está aumentando. Epidemia 
- R = 1: cada infectado causa uma nova infecção. Endemia
- R < 1: cada vez menos indivíduos se infectam e o número dos contágios retrocede.

Vamos verificar se podemos visualizar alguma relação entre mudanças no valor de
R e as diversas variantes:

![png](../projeto_covid_files/projeto_covid_84_0.png)
    
Valores elevados aparecem apenas no início do período. É possível perceber leves
aumentos coincidentes com os períodos de máximos de novos casos discutidos
anteriormente. Como se trata dos valores de todo o mundo, a curva pode estar
sendo balanceada entre países com taxas elevadas e países com taxas baixas.
Vamos refazer o gráfico com os dados dos cinco países com mais casos:
    
![png](../projeto_covid_files/projeto_covid_86_0.png)
    
Aqui vemos mais flutuações, especialmente no caso do Reino Unido. Os períodos de
maiores valores coincidem com os períodos de aumento de novos casos vistos
anteriormente.

Em um primeiro momento, podemos pensar se há alguma relação entre a taxa de
reprodução efetiva e a densidade populacional de um país. Afinal, mais pessoas
próximas pode levar à uma maior transmissibilidade. Vamos verificar os países
com maiores taxas de reprodução efetiva ao fim de setembro de 2021 e verificar
se há algum sinal de correlação:
    
![png](../projeto_covid_files/projeto_covid_89_0.png)
    
<iframe width="100%" height="500" frameborder="0" scrolling="no"
src="//plotly.com/~chicolucio/23.embed?autosize=true"></iframe>

Vemos que Singapura, localidade com segunda maior densidade populacional do
mundo, apresentava ao final de setembro de 2021 a maior taxa de reprodução. No
entanto, Mônaco, maior densidade, está com valor de R < 1. Ao menos pelo
gráfico, não parece haver uma correlação tão direta entre as duas variáveis mas,
como são dinâmicas, o ideal é fazer uma análise temporal e não estática como no
caso. Perceba, também, que a variável escolhida para o tamanho dos círculos foi
o total de casos por milhão em cada localidade. Veja que há localidades com
grande quantidade de casos acumulados mas com R < 1, indicando que já tiveram
momentos de maior contágio no passado mas que, em setembro de 2021, estavam em
momentos de retração de contágio.

## Evolução do número de mortes

<center><img alt="covid_banner" width="50%"
src="https://image.freepik.com/free-vector/coronavirus-cells-banner_1035-18749.jpg"></center>

Infelizmente alguns casos acabam levando a mortes. Vamos analisar como foi a
evolução dos números.

### Total de mortes

<!-- #### Mundo  -->

Começando pelo total de mortes no mundo:
    
![png](../projeto_covid_files/projeto_covid_94_0.png)
    

Observe que a ordem de grandeza dos eixos é bem menor que a vista no gráfico
análogo referente ao número de casos. Voltaremos a esse ponto posteriormente.

Vamos verificar os países que possuem os maiores totais de óbitos até setembro
de 2021:
    
![png](../projeto_covid_files/projeto_covid_99_0.png)
    
Vemos que o Brasil aparece em segundo, atrás apenas dos Estados Unidos. Também
observamos a presença de dois outros países da América do Sul entre os 10
países: o Peru e a Colômbia. Como tais países não possuem populações tão
numerosas quanto os primeiros colocados, é um sinal que a proporção de mortes
por milhão em tais países é alta, o que veremos mais adiante.

<!-- ##### Totais por milhão de habitantes -->

Como já discutido ao apresentar os dados referentes a casos de COVID-19, números
absolutos por vezes distorcem nossa percepção sobre os impactos reais em cada
localidade. Faz mais sentido avaliar os números por milhão de habitantes. 

![png](../projeto_covid_files/projeto_covid_104_0.png)
    
Percebe-se claramente que a situação do Peru é diferenciada. E tal situação vem
chamando a atenção [da mídia
mundial](https://www.bbc.com/news/world-latin-america-53150808) e de [periódicos
especializados](https://www.bmj.com/content/373/bmj.n1442). As condições
precárias em regiões mais pobres do país e problemas no setor de saúde parecem
contribuir para o alto número de óbitos. Da mesma forma, a situação do leste
europeu [tem gerado
manchetes](https://www.ft.com/content/06b30dfb-998e-443f-a2bd-41f0b2ca4ab9) nos
principais veículos de mídia mundiais.

Vamos analisar a situação de cada país por continente:

<iframe width="100%" height="500" frameborder="0" scrolling="no"
src="//plotly.com/~chicolucio/25.embed?autosize=true"></iframe>

Já fiz uma pequena discussão na parte de casos acerca de países que
provavelmente estão com dados abaixo dos números reais. Mas aqui cabem mais
algumas observações sobre este tópico.

Começando pela América do Sul, percebemos a quase ausência da Venezuela, com um
número muito abaixo de seus vizinhos. Obviamente que tal fato chamou a atenção
a ponto de a [Nature](https://www.nature.com/articles/d41586-021-02276-1) e o
periódico [BMJ](https://www.bmj.com/content/371/bmj.m3938) irem verificar a
situação. Como é de se esperar de regimes ditatoriais, há grande opressão
governamental para abafar os casos e óbitos, além de falta de recursos básicos
para controle da situação, conforme descrito nos links.

Na América do Norte, apesar de boa parte da atenção midiática ser destinada aos
Estados Unidos, vemos que o México possui maior número de óbitos por milhão de
habitantes.

Por fim, um pouco sobre a África e a Ásia. De acordo com as [Nações
Unidas](https://unstats.un.org/unsd/demographic-social/crvs/#coverage) parte
significativa dos países africanos e asiáticos não possuem registros de mortes
eficientes, conforme figura abaixo, retirada do site da instituição. Desta forma,
cuidado deve ser tomado ao se fazer comparações com estes países.

<center><img alt="covid_banner" width="60%"
src="https://unstats.un.org/unsd/demographic-social/crvs/documents/DeathCov.jpg"></center>

Há também [esta notícia](https://www.bbc.com/news/world-africa-55674139) a
respeito dos problemas de registro de óbitos em países africanos.

Vejamos um mapa interativo da evolução das mortes por milhão em cada país:

[![gif](../projeto_covid_files/projeto_covid_total_mortes_milhao.gif)](https://plotly.com/~chicolucio/27/)
<p style="font-size:60%;color:gray">
Caso queira interagir com o mapa, clique na animação que você será redirecionado
para uma plataforma onde pode verificar mais detalhes. Mas termine de ler o texto
primeiro ;-)</p>


### Novas mortes

Até o momento focamos em avaliar a evolução do total de óbitos no mundo e em
cada país. Agora, vamos verificar a evolução de novos óbitos, à medida que foram
sendo reportados. Assim, podemos verificar em quais momentos ocorreram as
popularmente chamadas "ondas".

A base de dados apresenta as entradas na forma de dados diários e na forma de
média móvel de 7 dias. A média móvel ajuda a visualizar melhor a tendência dos
dados, visto que há grande variação dos dados diários. Vamos começar com as
mortes em todo mundo:
    
![png](../projeto_covid_files/projeto_covid_116_0.png)
    
Vemos que os novos óbitos acompanham o perfil de novos casos já discutido
anteriormente, notando-se apenas uma grande diminuição na ordem de grandeza.
Desta forma, as mesmas discussões e relações observadas com as datas de
variantes e épocas de inverno se mantêm.

Da mesma maneira, as comparações com os países com mais casos e com os
continentes individuais feitas na discussão sobre os casos continuam válidas
para os óbitos. Assim como a sazonalidade observada para novos casos. 
Para o artigo não ficar muito longo, retirei os gráficos desta parte. Caso 
queira verificar todos os gráficos e análises, veja o repositório linkado ao 
fim do artigo.
    
## Evolução no número de vacinados

<center><img alt="covid_banner" width="50%"
src="https://image.freepik.com/free-photo/close-up-hand-holding-coronavirus-vaccine_23-2149012368.jpg"></center>

Certamente um dos pontos mais discutidos recentemente no contexto da pandemia.
Vamos verificar a evolução.

### Total de vacinados

Começando pelo total de vacinados:
    
![png](../projeto_covid_files/projeto_covid_129_0.png)
    
Os gráficos começam em dezembro de 2020, quando as primeiras doses foram
aplicadas ao redor do mundo. Vemos que, até setembro de 2021, cerca de 3,5
bilhões de pessoas já receberam ao menos uma dose da vacina, praticamente metade
da população mundial. Os saltos nos gráficos se devem a questões de
disponibilização dos dados.

Vejamos quantas pessoas já receberam todas as doses protocolares:
    
![png](../projeto_covid_files/projeto_covid_131_0.png)
    
Agora, quais países possuem mais pessoas vacinadas com ao menos uma dose:
    
![png](../projeto_covid_files/projeto_covid_134_0.png)
    
Vemos aqui que a Índia, considerando sua imensa população, já vacinou com ao
menos uma dose cerca de metade das pessoas. Brasil e Estados Unidos também
possuem quantidades significativas de pessoas vacinadas. Lembrando que não
necessariamente o número a ser buscado é 100 % tendo em vista que os protocolos
atuais não preveem vacinações em crianças.

Assim como para casos e óbitos, análises comparativas fazem mais sentido com
números relativos ao total da população de cada localidade. Vejamos, então, os
locais com maior percentual da população vacinada:
    
![png](../projeto_covid_files/projeto_covid_139_0.png)
    
Obviamente que o valor para Gibraltar está equivocado. [De acordo com a
OWID](https://ourworldindata.org/covid-vaccinations#frequently-asked-questions),
valores maiores do que 100 % podem ocorrer quando os valores para população
estão desatualizados e/ou quando pessoas não residentes nos locais acabam
entrando na conta como, por exemplo, turistas. No caso de Gibraltar, o aspecto
turístico pode estar causando essa distorção.

Como era de se esperar, vemos o predomínio de localidades relativamente pequenas
no que diz respeito ao número de habitantes. Vejamos um gráfico de países por
continente:

<iframe width="100%" height="500" frameborder="0" scrolling="no"
src="//plotly.com/~chicolucio/29.embed?autosize=true"></iframe>

Observe que o gráfico acima possui menos países que os mostrados anteriormente
para casos e óbitos. Isto se deve ao fato de que nem todos os países possuem
registros oficiais de vacinados de onde a OWID possa obter dados. O intervalo de
disponibilização dos dados também é distinto para cada país, de forma que
comparações devem ser feitas com cautela.

Vejamos se há alguma relação entre quantidade de vacinados e PIB per capita e se
países com pessoas mais velhas estão se vacinando mais:

<iframe width="100%" height="500" frameborder="0" scrolling="no"
src="//plotly.com/~chicolucio/31.embed?autosize=true"></iframe>

Como já discutido anteriormente, países com maior PIB per capita tendem a ter
uma população mais velha e, pelo gráfico, uma maior parcela da população
vacinada. Faz sentido se considerarmos que há custo envolvido em adquirir as
vacinas, de forma que países mais ricos tendem a conseguir adquirir vacinas mais
facilmente e a ter melhores condições de realizar grandes campanhas de
vacinação.

### Novas vacinações

Já vimos a evolução dos números totais, vejamos agora a evolução de novas
vacinações:
    
![png](../projeto_covid_files/projeto_covid_147_0.png)
    
Vemos que há uma tendência decrescente. Podemos considerar algumas hipóteses
para este declínio que, provavelmente, ocorrem em conjunto:

- a mais evidente é que à medida que mais pessoas completam o protocolo de
vacinação, menos vacinas são tomadas;
- países mais pobres estão tendo [dificuldades de adquirir
vacinas](https://www.nature.com/articles/d41586-021-02383-z). Conforme o artigo
do link, há pressão para que as patentes das tecnologias aplicadas pelas grandes
farmacêuticas sejam quebradas para que empresas locais possam fabricar as
vacinas a um menor custo. Ou que sejam feitas colaborações com as tais empresas;
- menos pessoas querendo se vacinar, por diversos motivos como (evidências nos
links):
    - a crescente percepção pública e de cientistas de que as [vacinas são menos
    eficazes](https://www.nature.com/articles/d41586-021-02532-4) do que
    inicialmente se pensava, especialmente devido ao surgimento de variantes;
    - crescente desconfiança quanto a potenciais conflitos de interesse que
    eventualmente contribuíram para [a aprovação emergencial das
    vacinas](https://www.bmj.com/content/373/bmj.n1283) com [triagem
    diferenciada](https://ncirs.org.au/phases-clinical-trials) e para o boicote
    a medicamentos utilizados no que ficou conhecido como [tratamento
    precoce](https://www.sciencedirect.com/science/article/pii/S2052297520300627);
    - crescente revolta entre a população de diversos países frente à imposição
    dos chamados passaportes de vacinação. Exemplos
    [aqui](https://www.foxnews.com/world/protesters-pack-paris-streets-in-defiance-of-covid-19-vaccine-passport-our-freedoms-are-dying)
    e [aqui](https://www.bmj.com/content/375/bmj.n2575).
    
Qual dos motivos apontados contribui mais ou se há mais algum motivo é difícil
de avaliar com base nos dados disponíveis.

## Comparações e correlações

<center><img alt="covid_banner" width="50%"
src="https://image.freepik.com/free-photo/close-up-pen-market-research_1098-3465.jpg"></center>

Já analisamos casos, óbitos e vacinações de forma separada. Vamos começar a
buscar comparações e correlações. Primeiro, vamos comparar a ordem de grandeza
entre casos, mortes e vacinações:
    
![png](../projeto_covid_files/projeto_covid_151_0.png)
    
Como já abordado brevemente anteriormente, há uma diferença de cerca de duas
ordens de grandeza entre casos e mortes. Podemos ver isto também em um gráfico
com os mesmos dados por milhão de habitantes:
    
![png](../projeto_covid_files/projeto_covid_153_0.png)
    
Esta observação está coerente com estudos que indicam que a mortalidade da
COVID-19 fica em torno de 1 a 2 %. Cabe ressaltar que há diferentes formas de
cálculo e interpretações para mortalidade [como mostra esse material da
OMS](https://www.who.int/news-room/commentaries/detail/estimating-mortality-from-covid-19),
especialmente com a pandemia em andamento. E já vimos que há diversas questões
envolvendo a disponibilização de dados por parte de alguns países. 

No entanto, se confirmada essa faixa de mortalidade, a pandemia de COVID-19
seria [proporcionalmente menos
fatal](https://www.news-medical.net/health/How-does-the-COVID-19-Pandemic-Compare-to-Other-Pandemics.aspx)
que outras pandemias no último século. O que diferencia a COVID-19, segundo
[estudos](https://tdtmvjournal.biomedcentral.com/articles/10.1186/s40794-020-00129-9),
é sua alta transmissibilidade que a espalhou por todo o planeta, de forma que a
contagem total de mortos se tornou elevada. Cabe ressaltar que, como toda
comparação, cuidados devem ser tomados e há
[estudos](https://www.cambridge.org/core/journals/infection-control-and-hospital-epidemiology/article/why-comparing-coronavirus-disease-2019-covid19-and-seasonal-influenza-fatality-rates-is-like-comparing-apples-to-pears/FDFE92B10895C57C42F59C2422DB7582)
que abordam tais cuidados, que fogem ao escopo deste trabalho.

Durante a análise, em alguns momentos correlações de casos e óbitos com idade da
população e PIB per capita foram estudadas. Vamos avaliar, através de [matrizes
de correlação](https://en.wikipedia.org/wiki/Correlation), se há correlações
positivas ou negativas entre variáveis. Comecemos procurando relações entre
totais de casos/óbitos com vacinação, indicadores de riqueza e indicadores de
idade:
    
![png](../projeto_covid_files/projeto_covid_156_0.png)
    
Podemos criar matriz similar mas para novos casos/óbitos/vacinações e suas
relações com índices que variam no tempo:
    
![png](../projeto_covid_files/projeto_covid_158_0.png)
    
Nas figuras acima temos algumas correlações mais óbvias e que já foram
discutidas no decorrer do estudo, que são:

- correlação positiva entre idades mais avançadas e maior PIB per capita;
- correlação positiva entre mais pessoas vacinadas e maior PIB per capita;
- como consequência das anteriores, correlação positiva entre pessoas vacinadas
e idades mais avançadas

Outras correlações:

- correlação positiva entre pessoas vacinadas e casos por milhão;
- correlação negativa entre pessoas vacinadas e mortes por milhão;
- correlação positiva entre o índice de restrição e a taxa de reprodução
efetiva;
- correlação positiva entre o índice de restrição e novos casos;
- correlação positiva entre o índice de restrição e novas mortes;

Estas três últimas correlações, embora fracas, encontram respaldo em algumas
publicações científicas recentes, como
[esta](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3711686) e
[esta](https://onlinelibrary.wiley.com/doi/10.1111/eci.13484), que parecem
indicar ineficácia ou mesmo piora no número de casos e óbitos devido ao uso de
medidas restritivas.

Muito se fala sobre o efeito de comorbidades, vamos verificar se há correlações
envolvendo as variáveis referentes a doenças:
    
![png](../projeto_covid_files/projeto_covid_160_0.png)
    
Curiosamente, há correlação negativa entre taxa de mortalidade por doenças
cardiovasculares (lembrando que os dados são de 2017) e casos e óbitos. E há
correlação positiva entre mulheres fumantes e casos e óbitos, assim como para
homens mas com valores menores. Não se pode fazer análises muito profundas com
base nestes dados pois não significam que as pessoas que tiveram os casos ou
morreram tinham as doenças ou fumavam. Os dados de fumantes e doenças são para
cada localidade e não para cada indivíduo.

## Brasil

<center><img alt="covid_banner" width="50%"
src="https://image.freepik.com/free-photo/realistic-shot-waving-flag-brazil-with-interesting-textures_181624-11214.jpg"></center>

Para terminar, vamos abordar um pouco da situação brasileira. Em alguns momentos
durante o estudo, comentários já foram feitos acerca do país, então apenas
complementaremos com mais informações. 

Comecemos verificando como a evolução de novos casos se relaciona com as datas
de surgimento de novas variantes e com os períodos de inverno:
    
![png](../projeto_covid_files/projeto_covid_164_0.png)
    
No gráfico há um aumento significativo de casos logo após a variante gama, cuja
primeira amostra foi no país. No entanto, cabe ressaltar que as variantes podem
demorar um certo tempo para se espalhar pelo mundo, de forma que as datas das
outras variantes não necessariamente são estas no país.

Há picos de casos nos invernos, porém o maior pico foi entre março e maio de 2021. 
E no último inverno tivemos um pico no início da estação mas, um perfil
decrescente na parte final.

Vejamos o perfil de novos óbitos:
    
![png](../projeto_covid_files/projeto_covid_166_0.png)
    
Vemos que o perfil de novos óbitos segue o de novos casos, mas em menor ordem de
grandeza.

Infelizmente a base de dados é bem incompleta para o Brasil. 
Há ausência de diversos dados referentes a hospitalizações e testes. Em
breve farei um estudo focado no país utilizando dados oficias governamentais.
Acompanhe meu [perfil no LinkedIn](https://www.linkedin.com/in/flsbustamante/)
para saber quando eu divulgar. 

## Conclusões


<center><img alt="covid_banner" width="50%"
src="https://image.freepik.com/free-photo/digital-world-map-hologram-blue-background_1379-901.jpg"></center>

Foi uma longa análise, mas espero que tenha sido proveitoso e que tenha trazido
novas visões a respeito da pandemia. Busquei colocar links para diversas
notícias e estudos com o intuito de embasar afirmações e facilitar a compreensão
por parte dos leitores. Vamos fazer uma breve recaptulação do que foi mostrado:

- evolução de casos e mortes e possíveis relações com variantes 
- sazonalidade dos casos e mortes e possível relação com períodos de inverno
- importância de avaliar os dados por milhão de habitantes ao invés dos totais
- correlações existentes entre diversas variáveis

Caso tenha dúvidas, comentários e/ou críticas construtivas me procure:

- [LinkedIn](https://www.linkedin.com/in/flsbustamante/)
- [GitHub](https://github.com/chicolucio)
- [Site](https://franciscobustamante.com.br/)
- [Telegram](https://t.me/chicolucio)

Veja o [repositório no
GitHub](https://github.com/chicolucio/panorama-covid-mundo) para ter acesso 
ao código e à análise completa. Ou, se preferir, clique no ícone abaixo
para abrir o trabalho completo no Google Colab:

[![Abra no Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/chicolucio/panorama-covid-mundo/blob/master/projeto_covid_colab.ipynb)

Conheça [meu portfolio](https://franciscobustamante.com.br/portfolio/).
