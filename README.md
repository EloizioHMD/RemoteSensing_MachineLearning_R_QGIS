# RemoteSensing_MachineLearning_R_QGIS

### Projeto de Geo Data Science sobre Classificação Supervisionada de Imagens do Sensoriamento Remoto de Satélites usando algorítimos de Machine Learning e Índices Espectrais.

![RemoteSensing_MachineLearning_R_QGIS](https://i.imgur.com/MZdanGV.png)<br>

Há muito tempo, em uma galáxia muito, muito distante… está escrito no código jedi das geociências, aindas nas primeiras linhas, “todo padawan em geociência deverá relatar por projetos aquilo que aprendeu de um mestre jedi em geociências por meio da forma que melhor comuniquem suas habilidade como cientista. Certamente esse código existe e em algum recanto das estrelas a força me guia neste momento. 

Recentemente, concluí a jornada "O Primeiro passo para se tornar um expert em Processamento de Imagens de Satélite no R", ministrado pelos instrutores Juliana Diniz e Daniel Maciel, na [RadarGeo](http://plataformaradargeo.com.br/curso_processamento_imagem/). Ao concluir o curso os instrutores encorajaram pela realização de uma projeto com os conhecimentos desenvolvidos ao longo do curso.

Essa proposta vem de encontro com os conteúdos lecionados no livro Geocomputation with R direcionado para quem deseja analisar, visualizar e modelar dados geográficos com software de código aberto baseado em R, uma linguagem de programação estatística que possui poderosos recursos de processamento de dados, visualização e geoespacial [(LOVELACE et al., 2019)](#love).

Lovelace agrupa termos em similaridade e tenta delimitar o escopo de uma ciências que surge das geotecnologias e vêm aproveitando toda evolução das Ciências de Dados. Nesse contexto, Geocomputação, Geographic Information Science (GIScience); Geomática; Geoinformática; Ciência da Informação Espacial; Engenharia de Geoinformação; e Geographic Data Science (GDS), são termos intimamente relacionados. Cada termo compartilha uma ênfase em uma abordagem "científica" (implicando reproduzível e falsificável) influenciada pelo GIS, embora suas origens e principais campos de aplicação sejam diferentes, porém as suas sobreposições entre os termos são maiores do que as diferenças entre eles [(LOVELACE et al., 2019)](#love).

Particularmente, gosto mais de Geographic Data Science (GDS), tem  mais a ver com o que irei comunicar em breve, além de estar bem hypado (modinha). Mas vou deixar um disclaimer para você leitor.

Ciência de dados é um campo interdisciplinar que usa métodos científicos, processos, algoritmos e sistemas para extrair conhecimento e insights de dados estruturados e não estruturados, e aplicar conhecimento e insights acionáveis de dados em uma ampla gama de domínios de aplicação [(DHAR,2013)](#dhar).

No entanto, a ciência de dados é diferente da ciência da computação e da ciência da informação, ou de qualquer outra ciência. O vencedor do Prêmio Turing, Jim Gray, imaginou a ciência de dados como um "quarto paradigma" das ciências (empírica, teórica, computacional e agora baseada em dados) e afirmou que "tudo na ciência está mudando por causa do impacto da tecnologia da informação" e do dilúvio de dados [(TANSLEY, 2009)](#tansley).

Como já enunciei, esse projeto de Geo Data Science irá relacionar dados do sensoriamento remoto de satélites imageadores, no caso Landsat 8, onde iremos realizar classificações supervisionadas por meio dos algoritmos de Machine Learning Support Vector Machine (SVM) e Random Forest. Iremos ao longo do projeto exploratória alguns índices espectrais para compreender se estes auxiliam na acurácia dos modelos.

<p align="center">
  <img src="https://i.kym-cdn.com/photos/images/newsfeed/001/297/055/875.gif">
</p>

Nas últimas décadas, o software livre e de código aberto para geoespacial ([FOSS4G](https://foss4g.org/)) progrediu a um ritmo surpreendente. Graças a organizações como a [OSGeo](https://www.osgeo.org/), a análise de dados geográficos não é mais um privilégio daqueles com hardware e software caros: qualquer pessoa agora pode baixar e executar bibliotecas espaciais de alto desempenho.

Os Sistemas de Informação Geográfica (GIS) de código aberto, como o [QGIS](https://qgis.org/pt_BR/site/), tornaram a análise geográfica acessível. Os programas GIS tendem a enfatizar as interfaces gráficas do usuário (GUIs), apesar de serem utilizados com linhas de comando (CLI). R, por outro lado, enfatiza a interface de linha de comando (CLI), que tem por melhor consequência a reprodutibilidade [(LOVELACE et al., 2019)](#love).

Algumas tarefas ainda são de difícil execução sem o auxílio de uma interface gráfica para essas irei usar o QGIS e ilustrar da melhor forma possível. As demais tarefas iremos usar o R baseando-se em [scripts que deixarei acessível desde já](https://github.com/EloizioHMD/r_script).

## Sobre a Área de Estudo

A área do estudo onde explorar por meio de dados geoespaciais são as cidades de Petrolina e Juazeiro, os dois maiores municípios da Região Administrativa Integrada de Desenvolvimento do Polo Petrolina e Juazeiro (RIDE Petrolina e Juazeiro), uma região administrativa que engloba oito municípios e uma população de mais de 700 mil habitantes numa área com cerca de 35 mil quilômetros quadrados ([Wikipédia, 2021](#wiki)).

Os municípios localizam-se na unidade geoambiental da Depressão Sertaneja, unidade que é formada pelas principais características do semiárido nordestino. Seu relevo é marcado por uma superfície de pediplanação muito monótona, sendo predominantemente suave-ondulado e atravessado por vales estreitos com vertentes dissecadas. Na linha do horizonte também pontuam elevações residuais, cristas com/sem outeiros. Esse tipo de relevo é testemunha dos ciclos intensos de erosão que atingiram o sertão nordestino ([Wikipédia, 2021](#wiki)).

O clima é classificado como semiárido quente (do tipo BSh na classificação climática de Köppen-Geiger), com regime de chuvas de primavera-verão. Este clima é caracterizado pela escassez e irregularidade de chuvas, assim como a forte evaporação por conta das altas temperaturas. A temperatura média compensada anual é de 26,9 °C, possuindo verões quentes e mais úmidos e invernos mornos e secos. O índice pluviométrico é de apenas 483 milímetros por ano (mm/ano), um dos mais baixos do Brasil, com um tempo de insolação de quase 3 000 horas anuais ([Wikipédia, 2021](#wiki)).

<p align="center">
  <img src="https://thumbs.gfycat.com/DarlingMediumCanvasback-size_restricted.gif">
</p>

Estas informações precisam ser levantadas antes da aquisição das imagens de satélite, pois fornecem insight sobre os alvos imageados pelos sensores ajudando na escolha das cenas e posterior elaboração das análises.

## Aquisição de Dados Geoespaciais Necessários

### Dados de Satélite

Diversos serviços disponibilizam dados do sensoriamento remoto de forma gratuíta, entre eles os principais são: [Earth Explorer](https://earthexplorer.usgs.gov/), que pertence ao serviço mantenedor da missão Landsat; e [Scihub](https://scihub.copernicus.eu/dhus/#/home) que pertence ao serviço mantenedor.  Ainda é oportuno destacar o serviço do INPE onde é possível baixar os produtos da missão [CBERS](http://www.dgi.inpe.br/CDSR/).

Cada sensor possui suas características, no caso iremos usar dados obtidos pelo Operational Land Imager (OLI) - Construído pela Ball Aerospace & Technologies Corporation é composto por 9 bandas espectrais, incluindo uma banda pan:
> Banda 1 visível (0,43 - 0,45 µm) 30 m<br>
> Banda 2 visível (0,450 - 0,51 µm) 30 m<br>
> Banda 3 visível (0,53 - 0,59 µm) 30 m<br>
> Banda 4 vermelha (0,64 - 0,67 µm) 30 m<br>
> Banda 5 infravermelho próximo (0,85 - 0,88 µm) 30 m<br>
> Banda 6 SWIR 1 (1,57 - 1,65 µm) 30 m<br>
> Banda 7 SWIR 2 (2,11 - 2,29 µm) 30 m<br>
> Banda 8 Pancromática (PAN) (0,50 - 0,68 µm) 15 m<br>
> Banda 9 Cirrus (1,36 - 1,38 µm) 30 m<br>

Sensor infravermelho térmico (TIRS) - Construído pela NASA Goddard Space Flight Center possuir duas bandas espectrais:
> Banda 10 TIRS 1 (10,6 - 11,19 µm) 100 m<br>
> Banda 11 TIRS 2 (11,5 - 12,51 µm) 100 m<br>

A USGS adota algumas nomenclaturas para detalhar seu produto. Assim, iremos baixar o produto Landsat 8 da collection 2 e no Level 2. O que isso significa? Iremos baixar um dado com uma segunda camada de pré-processamento, na qual a reflectância da superfície fornece uma estimativa da reflectância espectral da superfície da Terra, uma vez que seria medida ao nível do solo na ausência de dispersão ou absorção atmosférica e são gerados a partir do Código de reflectância da superfície da terra (LaSRC)([MASEK, et al., 2020](#masek))

<p align="center">
  <img src="https://github.com/EloizioHMD/RemoteSensing_MachineLearning_R_QGIS/blob/main/arquivos/img/Captura%20de%20tela%20de%202021-06-03%2000-33-15.png?raw=true">
</p>

Então, no Earth Explorer, após cadastra-se junto ao USGS, poderá usar a ferramenta marque a área de interesse sob o mapa, no período com menor ocorrência de chuvas outono e inverno, como pesquisamos e em dataset selecione landsat 8 collection 2 level 2.
* LC08_L2SP_217066_20201008_20201016_02_T1.tar
* LC08_L2SP_217067_20201008_20201016_02_T1.tar

Se quebrarmos esse nomme de arquivos termos muita informações sobre os dados ([USGS, 2021](#usgs)):

> LC08 - Trata-se de uma produto Landsat 8<br>
> L2SP - De level 2 de Science Product<br>
> 217067 - Coluna e linha do orbital WRC-2<br>
> 20201008 - Data de aquisição do dado<br>
> 20201016 - Data de processamento do dado<br>
> 02_T1 - Coleção 2 tier 1<br>

Para facilitar esse dado processado pode ser baixado neste link ou na pasta arquivos.

### Dados Vetorial

A malha das áreas de todos os municípios podem ser adquiridas junto aos [geoserviços do IBGE](https://www.ibge.gov.br/geociencias/organizacao-do-territorio/malhas-territoriais/15774-malhas.html?=&t=acesso-ao-produto). Assim, usamos o QGIS para salvar as feições selecionadas (município de petrolina e juazeiro) e posteriormente usamos a ferramenta de vetor para dissolver camada transformando a camada de multipolygon para polygon. Para facilitar esse dado processado pode ser baixado [neste link](https://github.com/EloizioHMD/RemoteSensing_MachineLearning_R_QGIS/blob/main/arquivos/area_disolver.zip) ou na pasta arquivos.

## Finalmente CLI

Para começar a geocomputar com R é preciso instalar o R base, que pode ser baixado no site do [R-Project](https://www.r-project.org/). É possível trabalhar diretamente no R instalado por base, mas não é nada prático. Para isso, para explorar de forma mais ampla as potencialidades do R recomenda-se a instalação do [R-Studio](https://www.rstudio.com/). Com isso poderemos iniciar a “codar”. 

<p align="center">
  <img src="https://memegenerator.net/img/instances/62332220/embrace-the-power-of-the-command-line.jpg">
</p>

### Carregando os pacotes
Iremos precisar de alguns pacotes para instalação de qualquer pacote no R basta usar o comando `install.packages("nome-do-pacote")` depois de instalado podemos usar o comando `require()` ou `library()`. Assim, vamos implementar uma rotina para requerer um pacote e o R nos retorne se já está instalado (`TRUE`) ou não (`FALSE`).

```{r}
pkg <- c("raster", "rgdal", "rgeos", "sf")
sapply(pkg, require, character.only = T)
```
Caso não tenha basta dar um install.packages rodar novamente a rotina até tudo sair `TRUE`.
```{r}
install.packages("raster")
install.packages("rgdal")
install.packages("rgeos")
install.packages("sf")
```

### Carregando dados raster
Para carregar os dados usaremos comando `list.files` e ter acesso as pastas descompactadas dos arquivos. Note que o arquivo compatado entrega diversos arquivos. Usaremos apenas os arquivos .tif referentes as bandas 1 a 7.

Para otimizar essa seleção iremos buscar um padrão usando o `glob2rx()`, que utiliza das expressões regulares para retornar um padrão. Assim, se olharmos os arquivos que desejamos `LC08_L2SP_217066_20201008_20201016_02_T1_SR_B2.TIF` o trecho que se repete é o 'SR_B', sendo essa a expressão regular que desejamos.

```{r}
options(stringsAsFactors = FALSE)

all_band_r1 <- list.files("R/GDS/raster/LC08_L2SP_217066_20201008_20201016_02_T1_SR/", 
                       pattern = glob2rx("*SR_B*.TIF$"), full.names = T)
l8c_r1 <- stack(all_band_r1)
all_band_r2 <- list.files("R/GDS/raster/LC08_L2SP_217067_20201008_20201016_02_T1_SR/",
                       pattern = glob2rx("*SR_B*.TIF$"), full.names = T)
l8c_r2 <- stack(all_band_r2)

l8c_mosaico <- mosaic(l8c_r1, l8c_r2, fun=mean)
```
O que o `options(stringsAsFactors = FALSE)` é desligar a conversão global de strings em fatores.

Na sequência o código colocou em um objeto `all_band_rn` a lista de arquivos selecionados pelo padrão definido. Depois usamos esse objeto para criar um `stack()`, que é um classe raster e atribuímos ao objeto `l8c_rn`. Como não iremos trabalhar individualmente cada um desses raster, mas sim de forma única o comando `mosaic()` realiza o mosaíco das imagens.

Para visualizar o resultado apenas como forma de conferir usamos o `plot(l8c_mosaico$layer.5)`, que é uma forma rápida de realizar essa atividade.

<p align="center">
  <img src="https://raw.githubusercontent.com/EloizioHMD/RemoteSensing_MachineLearning_R_QGIS/main/arquivos/img/plot_mosaic_nir_band5.png">
</p>

### Carregando dados vetoriais
O dado a ser carregado está no formato shapefile. Precisamos carregar apenas o arquivo que contem a geometria, que é o arquivo .shp usando o `readOGR`.
```{r}
area_int <- readOGR("R/GDS/vector/area_disolver.shp")
```
Para visualizar o resultado apenas como forma de conferir pode ser usado o `plot(area_int)`.

### Transformação do Sistema de Referência e Coordenadas

Os objetos carregados são de duas classes destintas, vetor e raster, mas ambos são dados geoespaciais possuindo portanto um sistema de referencia e coordenadas (CRS) a qual o dado é projetado. Vejamos:
```{r}
> crs(area_int)
CRS arguments:
 +proj=longlat +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +no_defs 
> crs(l8c_mosaico)
CRS arguments:
 +proj=utm +zone=24 +datum=WGS84 +units=m +no_defs 
 ```
Como usaremos mais adiante o vetor como mascará para recortar o raster para área de interesse precisamos deixar ambos no mesmo CRS. Podemos fazer isso por meio do seguinte código:
```{r}
area_int_utm <- spTransform(x = area_int, CRSobj = crs(l8c_mosaico))
```

### Aplicando Mascará(mask) e Recorte(crop)
Como antecipei iremos aplicar sob o raster o dado vetor para "recortar" apenas os da área de interesse pelo comando `mask()`, sendo 'x' o raster e 'mask' o vetor.
Porém, o raster nada mais é que uma matrix com valores dos pixeis, assim o que a função `mask()` faz é apagar os valores que não estão sobre a area nascarada, que apesar de não possuir a informação do pixel mantém a matriz do mesmo tamanho. Logo, para reduzirmos o tamanho da matriz usamos `crop()`.
```{r}
area_int_mask <- mask(x = l8c_mosaico, mask = area_int_utm)
area_int_crop <- crop(area_int_mask, area_int_utm)
```
Essas operação exigem um pouco mais do hardware e pode demorar mais que as outras. 

### Renomear camada e salvar arquivo um
A renomeação das camadas é simples, o código basta por o código que chama o nome das camadas como objeto e inserir um conjunto de valores para substituí-los.
```{r}
names(area_int_crop) <- c("B1", "B2", "B3", "B4", "B5", "B6", "B7")
```

...つづく

### Referências:

> <a name="love">LOVELACE, Robin; NOWOSAD, Jakub; MUENCHOW, Jannes</a>. [Geocomputation with R](https://geocompr.robinlovelace.net/). CRC Press, 2019.<br>

> <a name="dhar">DHAR, Vasant</a>. [Data science and prediction. Communications of the ACM](https://dl.acm.org/doi/abs/10.1145/2500499), v. 56, n. 12, p. 64-73, 2013.<br>

> <a name="tansley">HEY, Tony; TANSLEY, Stewart; TOLLE, Kristin.</a> [O Quarto paradigma: descobertas científicas na era da eScience](https://books.google.com.br/books?hl=pt-BR&lr=&id=bVm_4X4Wm-UC&oi=fnd&pg=PT4&dq=o+quarto+paradigma&ots=Pzplg6bdlr&sig=RtQy80y4bLEvU60NbgLOVckO4AY#v=onepage&q=o%20quarto%20paradigma&f=false). Oficina de Textos, 2011.<br>

> <a name="wiki">PETROLINA. In: WIKIPÉDIA</a>, a enciclopédia livre. Flórida: Wikimedia Foundation, 2021. Disponível em: <https://pt.wikipedia.org/w/index.php?title=Petrolina&oldid=61213019>. Acesso em: 22 mai. 2021.<br>

> <a name="masek">MASEK, Jeffrey G. et al</a>. Landsat 9: Empowering open science and applications through continuity. Remote Sensing of Environment, v. 248, p. 111968, 2020.<br>

> <a name="usgs">United States Geological Survey, USGS</a>. Landsat Collection 2 Level-2 Science Products, 2021. Disponível em: <https://www.usgs.gov/core-science-systems/nli/landsat/landsat-collection-2-level-2-science-products>. Acesso em: 02 jun. 2021.<br>
