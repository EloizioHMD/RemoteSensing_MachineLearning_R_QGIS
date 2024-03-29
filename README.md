# RemoteSensing_MachineLearning_R_QGIS

### Projeto de Geo Data Science sobre Classificação Supervisionada de Imagens do Sensoriamento Remoto de Satélites usando algorítimos de Machine Learning e Índices Espectrais.

![RemoteSensing_MachineLearning_R_QGIS](https://i.imgur.com/MZdanGV.png)<br>

Há muito tempo, em uma galáxia muito, muito distante… está escrito no código jedi das geociências, aindas nas primeiras linhas, “todo padawan em geociência deverá relatar por projetos aquilo que aprendeu de um mestre jedi em geociências, por meio da forma que melhor comuniquem suas habilidade como cientista". Certamente esse código existe e em algum recanto das estrelas a força me guia neste momento. 

Recentemente, concluí a jornada "O Primeiro passo para se tornar um expert em Processamento de Imagens de Satélite no R", ministrado pelos instrutores Juliana Diniz e Daniel Maciel, na [RadarGeo](http://plataformaradargeo.com.br/curso_processamento_imagem/). Ao concluir o curso os instrutores encorajaram pela realização de uma projeto com os conhecimentos desenvolvidos ao longo do curso.

A proposta do referido curso, coincide com outra das minhas referências na área, o livro *Geocomputation with R*, de Robin Lovelace, Jakub Nowosad e Jannes Muenchow. Com lições para quem deseja analisar, visualizar e modelar dados geográficos com R, uma linguagem de programação estatística que possui poderosos recursos de processamento de dados, visualização e geoespacial [(LOVELACE et al., 2019)](#love).

Lovelace agrupa termos em similaridade e tenta delimitar o escopo de uma ciências que surge das geotecnologias e vêm aproveitando toda evolução das Ciências de Dados. Nesse contexto, Geocomputação, Geographic Information Science (GIScience); Geomática; Geoinformática; Ciência da Informação Espacial; Engenharia de Geoinformação; e Geographic Data Science (GDS), são termos intimamente relacionados. Cada termo compartilha uma ênfase em uma abordagem "científica" (implicando reproduzível e falsificável) influenciada pelo GIS, embora suas origens e principais campos de aplicação sejam diferentes, as sobreposições entre os termos são maiores do que as diferenças entre eles [(LOVELACE et al., 2019)](#love).

Particularmente, gosto mais do termo Geographic Data Science (GDS), tem  mais a ver com o que irei comunicar a seguir, além de estar bem hypado (modinha). Mas vou deixar um disclaimer para você leitor.

Ciência de dados é um campo interdisciplinar que usa métodos científicos, processos, algoritmos e sistemas para extrair conhecimento e insights de dados estruturados e não estruturados, e aplicar conhecimento e insights acionáveis de dados em uma ampla gama de domínios de aplicação [(DHAR,2013)](#dhar).

No entanto, a ciência de dados é diferente da ciência da computação e da ciência da informação, ou de qualquer outra ciência. O vencedor do Prêmio Turing, Jim Gray, imaginou a ciência de dados como um "quarto paradigma" das ciências (empírica, teórica, computacional e agora baseada em dados) e afirmou que "tudo na ciência está mudando por causa do impacto da tecnologia da informação" e do dilúvio de dados [(TANSLEY, 2009)](#tansley).

Como já enunciei, esse projeto de Geo Data Science relacionará dados do sensoriamento remoto de satélites imageadores, no caso Landsat 8, onde realizaremos  classificações supervisionadas por meio de dois algoritmos de Machine Learning, Support Vector Machine (SVM) e Random Forest. Também será verificada a influência que os índices espectrais exercem na acurácia dos modelos.

<p align="center">
  <img src="https://i.kym-cdn.com/photos/images/newsfeed/001/297/055/875.gif">
</p>

Nas últimas décadas, o software livre e de código aberto para geoespacial ([FOSS4G](https://foss4g.org/)) progrediu a um ritmo surpreendente. Graças a organizações como a [OSGeo](https://www.osgeo.org/), a análise de dados geográficos não é mais um privilégio daqueles com hardware e software caros: qualquer pessoa agora pode baixar e executar bibliotecas espaciais de alto desempenho.

Os Sistemas de Informação Geográfica (GIS) de código aberto, como o [QGIS](https://qgis.org/pt_BR/site/), tornaram a análise geográfica acessível. Os programas GIS tendem a enfatizar as interfaces gráficas do usuário (GUIs), apesar de serem utilizados com linhas de comando (CLI). R, por outro lado, enfatiza a interface de linha de comando (CLI), que tem por melhor consequência a reprodutibilidade [(LOVELACE et al., 2019)](#love).

Algumas tarefas ainda são de difícil execução sem o auxílio de uma interface gráfica. Nessa dificuldade será utilizado o software QGIS e tentarei ilustrar da melhor forma possível as etapas realizadas para a reprodutibilidade dos resultados. As demais tarefas usaremos o R baseando-se em [scripts que deixarei acessível desde já](https://github.com/EloizioHMD/r_script).

## Sobre a Área de Estudo

A área do estudo onde explorar por meio de dados geoespaciais são as cidades de Petrolina e Juazeiro, os dois maiores municípios da Região Administrativa Integrada de Desenvolvimento do Polo Petrolina e Juazeiro (RIDE Petrolina e Juazeiro), uma região administrativa que engloba oito municípios e uma população de mais de 700 mil habitantes numa área com cerca de 35 mil quilômetros quadrados ([Wikipédia, 2021](#wiki)).

Os municípios localizam-se na unidade geoambiental da Depressão Sertaneja, unidade que é formada pelas principais características do semiárido nordestino. Seu relevo é marcado por uma superfície de pediplanação muito monótona, sendo predominantemente suave-ondulado e atravessado por vales estreitos com vertentes dissecadas. Na linha do horizonte também pontuam elevações residuais, cristas com/sem outeiros. Esse tipo de relevo é testemunha dos ciclos intensos de erosão que atingiram o sertão nordestino ([Wikipédia, 2021](#wiki)).

O clima é classificado como semiárido quente (do tipo BSh na classificação climática de Köppen-Geiger), com regime de chuvas de primavera-verão. Este clima é caracterizado pela escassez e irregularidade de chuvas, assim como a forte evaporação por conta das altas temperaturas. A temperatura média compensada anual é de 26,9 °C, possuindo verões quentes e mais úmidos e invernos mornos e secos. O índice pluviométrico é de apenas 483 milímetros por ano (mm/ano), um dos mais baixos do Brasil, com um tempo de insolação de quase 3.000 horas anuais ([Wikipédia, 2021](#wiki)).

<p align="center">
  <img src="https://thumbs.gfycat.com/DarlingMediumCanvasback-size_restricted.gif">
</p>

Estas informações precisam ser levantadas antes da aquisição das imagens de satélite, pois fornecem insight sobre os alvos imageados pelos sensores ajudando na escolha das cenas e posterior elaboração das análises.

## Aquisição de Dados Geoespaciais Necessários

### Dados de Satélite

Diversos serviços disponibilizam dados do sensoriamento remoto de forma gratuita, entre eles os principais são: [Earth Explorer](https://earthexplorer.usgs.gov/), que pertence ao serviço mantenedor da missão Landsat; e [Scihub](https://scihub.copernicus.eu/dhus/#/home) que pertence ao serviço mantenedor. Ainda é oportuno destacar o serviço do INPE onde é possível baixar os produtos da missão [CBERS](http://www.dgi.inpe.br/CDSR/).

Cada sensor possui suas características, no caso usaremos dados obtidos pelo Operational Land Imager (OLI) - Construído pela Ball Aerospace & Technologies Corporation é composto por 9 bandas espectrais, incluindo uma banda pan:
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

A USGS adota algumas nomenclaturas para detalhar seu produto. Assim, baixaremos o produto Landsat 8 da collection 2 e no Level 2. O que isso significa? Será baixando um dado com uma segunda camada de pré-processamento, na qual a reflectância da superfície fornece uma estimativa da reflectância espectral da superfície da Terra, uma vez que seria medida ao nível do solo na ausência de dispersão ou absorção atmosférica e são gerados a partir do Código de reflectância da superfície da terra (LaSRC)([MASEK, et al., 2020](#masek))

<p align="center">
  <img src="https://github.com/EloizioHMD/RemoteSensing_MachineLearning_R_QGIS/blob/main/arquivos/img/Captura%20de%20tela%20de%202021-06-03%2000-33-15.png?raw=true">
</p>

Então, no Earth Explorer, após cadastra-se junto ao USGS, poderá usar a ferramenta marque a área de interesse sob o mapa, no período com menor ocorrência de chuvas outono e inverno, como pesquisamos e em dataset selecione landsat 8 collection 2 level 2.
* LC08_L2SP_217066_20201008_20201016_02_T1.tar
* LC08_L2SP_217067_20201008_20201016_02_T1.tar

Se quebrarmos esse nome de arquivos termos muita informações sobre os dados ([USGS, 2021](#usgs)):

> LC08 - Trata-se de uma produto Landsat 8<br>
> L2SP - De level 2 de Science Product<br>
> 217067 - Coluna e linha do orbital WRC-2<br>
> 20201008 - Data de aquisição do dado<br>
> 20201016 - Data de processamento do dado<br>
> 02_T1 - Coleção 2 tier 1<br>

Como os tamanhos dos arquivos excedem o limite máximo de tamanho de 100 MB, a quem desejar reproduzir será necessário acessar o site da EarthExplorer para aquisição dos dados.

### Dados Vetorial

A malha das áreas de todos os municípios podem ser adquiridas junto aos [geoserviços do IBGE](https://www.ibge.gov.br/geociencias/organizacao-do-territorio/malhas-territoriais/15774-malhas.html?=&t=acesso-ao-produto). Assim, usamos o QGIS para salvar as feições selecionadas (município de petrolina e juazeiro) e posteriormente usamos a ferramenta de vetor para dissolver camada transformando a camada de multipolygon para polygon. Para facilitar esse dado processado pode ser baixado [neste link](https://github.com/EloizioHMD/RemoteSensing_MachineLearning_R_QGIS/blob/main/arquivos/area_disolver.zip) ou na pasta arquivos.

## Finalmente CLI

Para começar a geocomputar com R é preciso instalar o R base, que pode ser baixado no site do [R-Project](https://www.r-project.org/). É possível trabalhar diretamente no R instalado por base, mas não é nada prático. Para isso, para explorar de forma mais ampla as potencialidades do R recomenda-se a instalação do [R-Studio](https://www.rstudio.com/). Com isso poderemos iniciar a “codar”. 

<p align="center">
  <img src="https://memegenerator.net/img/instances/62332220/embrace-the-power-of-the-command-line.jpg">
</p>

### Carregando os pacotes
Precisaremos de alguns pacotes para instalação de qualquer pacote no R basta usar o comando `install.packages("nome-do-pacote")` depois de instalado podemos usar o comando `require()` ou `library()`. Assim, vamos implementar uma rotina para requerer um pacote e o R nos retorne se já está instalado (`TRUE`) ou não (`FALSE`).

```R
pkg <- c("raster", "rgdal", "rgeos", "sf")
sapply(pkg, require, character.only = T)
```
Caso não tenha basta dar um install.packages rodar novamente a rotina até tudo sair `TRUE`.
```R
install.packages("raster")
install.packages("rgdal")
install.packages("rgeos")
install.packages("sf")
```

### Carregando dados raster
Para carregar os dados usaremos comando `list.files` e ter acesso as pastas descompactadas dos arquivos. Note que o arquivo compactado entrega diversos arquivos. Usaremos apenas os arquivos .tif referentes as bandas 1 a 7.

Para otimizar essa seleção buscaremos um padrão usando o `glob2rx()`, que se utiliza das expressões regulares para retornar um padrão. Assim, se olharmos os arquivos que desejamos `LC08_L2SP_217066_20201008_20201016_02_T1_SR_B2.TIF` o trecho que se repete é o 'SR_B', sendo essa a expressão regular que desejamos.

```R
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

Na sequência o código colocou em um objeto `all_band_rn` a lista de arquivos selecionados pelo padrão definido. Depois usamos esse objeto para criar um `stack()`, que é uma classe raster e atribuímos ao objeto `l8c_rn`. Como não trabalharemos individualmente cada um desses raster, mas sim de forma única, usaremos o `mosaic()` para realizar o mosaico das imagens.

Para visualizar o resultado apenas como forma de conferir usamos o `plot(l8c_mosaico$layer.5)`, que é uma forma rápida de realizar essa atividade.

<p align="center">
  <img src="https://raw.githubusercontent.com/EloizioHMD/RemoteSensing_MachineLearning_R_QGIS/main/arquivos/img/plot_mosaic_nir_band5.png">
</p>

### Carregando dados vetoriais
O dado a ser carregado está no formato shapefile. Precisamos carregar apenas o arquivo que contem a geometria, que é o arquivo .shp, usando o `readOGR`.
```R
area_int <- readOGR("R/GDS/vector/area_disolver.shp")
```
Para visualizar o resultado apenas como forma de conferir pode ser usado o `plot(area_int)`.

### Transformação do Sistema de Referência e Coordenadas

Os objetos carregados são de duas classes distintas, vetor e raster, mas ambos são dados geoespaciais possuindo portanto um sistema de referência e coordenadas (CRS) a qual o dado é projetado. Vejamos:
```R
> crs(area_int)
CRS arguments:
 +proj=longlat +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +no_defs 
> crs(l8c_mosaico)
CRS arguments:
 +proj=utm +zone=24 +datum=WGS84 +units=m +no_defs 
 ```
Como usaremos mais adiante o vetor como mascará para recortar o raster para área de interesse precisamos deixar ambos no mesmo CRS. Podemos fazer isso por meio do seguinte código:
```R
area_int_utm <- spTransform(x = area_int, CRSobj = crs(l8c_mosaico))
```

### Aplicando Mascará(mask) e Recorte(crop)
Como antecipei aplicaremos sob o raster o dado vetor para “recortar” apenas os da área de interesse pelo comando `mask()`, sendo 'x' o raster e 'mask' o vetor.
Porém, o raster nada mais é que uma matrix com valores dos pixeis, assim o que a função `mask()` faz é apagar os valores que não estão sobre a área mascarada, que apesar de não possuir a informação do pixel mantém a matriz do mesmo tamanho. Logo, para reduzirmos o tamanho da matriz usamos `crop()`.
```R
area_int_mask <- mask(x = l8c_mosaico, mask = area_int_utm)
area_int_crop <- crop(area_int_mask, area_int_utm)
```
Essas operações exigem um pouco mais do hardware e pode demorar mais que as outras. 

### Renomear camada e salvar arquivo um
A renomeação das camadas é simples, basta pôr o código que chama os nomes das camadas como objeto e inserir uma lista de valores de mesma dimensão para substituí-los
```R
names(area_int_crop) <- c("B1", "B2", "B3", "B4", "B5", "B6", "B7")
```
Essa é uma função de substituição genérica, não substituindo os valores em definitivo. Isso tem uma consequência que é para reaproveitar esse dado no futuro deve acompanhar um arquivo .csv contendo essa lista, tal qual o código seguinte.
```R
writeRaster(x = area_int_crop, filename = "R/GDS/saida/L8_B1B7.tif")
names_area <- names(area_int_crop)
write.csv(x = names_area, file = "R/GDS/saida/L8_B1B7.csv")
```
Para nossas experiências de testar o desempenho dos algorítimos de classificação com e sem índices espectrais salvaremos esse arquivo que servirá para essas análises futuras.

### Aritmética da Banda para Índices Espectrais
Primeiramente vamos atribuir a um novo o objeto o raster criado por meio dos dados geoespaciais.
```R
indices <- area_int_crop
names(indices) <- c("B1", "B2", "B3", "B4", "B5", "B6", "B7")
```
Acho que aqui cabe um "disclaimer", as imagens de satélites nada mais são que a aquisição de dados do imageamento das feições terrestres registradas pelo sensor através das medições da radiação eletromagnética da luz solar refletida da superfície de qualquer objeto ([MENESES; ALMEIDA, 2012](#mene)). 

A radiação ao interagir com algum objeto, em geral, parte absorvida é transformada em calor ou em algum outro tipo de energia e a parte refletida se espalha pelo espaço. O fator que mede a capacidade de um objeto de refletir a energia radiante indica a sua reflectância, enquanto que a capacidade de absorver energia radiante é indicada pela sua absortância e, da mesma forma, a capacidade de transmitir energia radiante é indicada pela sua transmitância ([MENESES; ALMEIDA, 2012](#mene)). 

Podemos medir a reflectância de um objeto para cada tipo de radiação que compõe o espectro eletromagnético e então perceber, através dessa experiência, que a reflectância de um mesmo objeto pode ser diferente para cada tipo de radiação que o atinge. Se olharmos o comportamento da reflectância do objeto para cada comprimento de onda teremos um conjunto de distribuição normal que é geralmente denominada por assinatura espectral ([MENESES; ALMEIDA, 2012](#mene)). 

Nesse escopo é possível aplicar álgebra sobre esses valores para melhora a visualização do comportamento espectral dos alvos.

**Razão simples (SR)**

É um índice de vegetação comum para estimar a quantidade de vegetação. É a proporção da luz espalhada no NIR e absorvida nas faixas vermelhas, que reduz os efeitos da atmosfera e da topografia. 

```R
indices$Simple_Ratio <- indices$B5 / indices$B4
```

**Índice de diferença de vegetação normalizada (NDVI)**

É um índice padronizado que permite gerar uma imagem exibindo o verde (biomassa relativa). Este índice aproveita o contraste das características de duas bandas de um conjunto de dados raster multiespectral - a absorção do pigmento de clorofila na banda vermelha e a alta refletividade dos materiais vegetais na banda NIR.

```R
indices$NDVI <- (indices$B5 - indices$B4)/(indices$B5 + indices$B4)
```

**Índice de Vegetacao Ajustado ao Solo (SAVI)**

O SAVI é um índice de vegetação que tenta minimizar as influências do brilho do solo usando um fator de correção do brilho do solo. Isso é frequentemente usado em regiões áridas onde a cobertura vegetal é baixa e produz valores entre -1,0 e 1,0. O fator de correção do brilho do solo (L), que varia dependendo da quantidade de cobertura vegetal verde. Em áreas sem cobertura vegetal verde, L = 1; em áreas de cobertura vegetal verde moderada, L = 0,5; e em áreas com cobertura vegetal muito alta, L = 0, que é equivalente ao método NDVI.

```R
indices$SAVI <- ((1 + 0.5) * (indices$B5 - indices$B4))/((indices$B5 + indices$B4) + 0.5)
```

**Índice de Área Foliar**

O IAF é um índice biofísico definido pela razão entre a área foliar de uma vegetação por unidade de área utilizada por esta vegetação, sendo um indicador da biomassa de cada pixel da imagem.

```R
indices$AFI <- log((0.69 - indices$SAVI)/(0.59))/0.91
```

**Índice de vegetação melhorada (EVI)**

O método EVI é um índice de vegetação otimizado que leva em consideração as influências atmosféricas e o sinal de fundo da vegetação. É semelhante ao NDVI, mas é menos sensível ao ruído de fundo e atmosférico, e não se torna tão saturado quanto o NDVI ao visualizar áreas com vegetação verde muito densa.

```R
indices$EVI <- 2.5 * ((indices$B5 - indices$B4)/(indices$B5 + 6 * indices$B4 - 7.5 * indices$bB2 + 1))
```

**Indice de agua da diferenca normalizada (NDWI)**

O NDWI é um índice para delinear e monitorar as mudanças de conteúdo nas águas superficiais. É calculado com o NIR e as faixas verdes.

```R
indices$NDWI <- (indices$B3 - indices$B5)/(indices$B3 + indices$B5)
```

### Plotando gráfico dos Índices em Tons de Cinza 

Vamos gerar um gráfico plotando todos os índices calculados demonstrados em tons de cinza, apenas para verificar a saída.

```R
par(mfrow = c(2,3))
plot(indices$Simple_Ratio, col = gray(0:100/100), main = "SR")
plot(indices$NDVI, col = gray(0:100/100), main = "NDVI")
plot(indices$SAVI, col = gray(0:100/100), main = "SAVI")
plot(indices$AFI, col = gray(0:100/100), main = "AFI")
plot(indices$EVI, col = gray(0:100/100), main = "EVI")
plot(indices$NDWI, col = gray(0:100/100), main = "NDWI")
```
<p align="center">
  <img src="https://raw.githubusercontent.com/EloizioHMD/RemoteSensing_MachineLearning_R_QGIS/main/arquivos/img/indices_area.png">
</p>

Aqui já detectamos o primeiro problema, o EVI não funcionou. Isso tende a ocorrer quando o modelo não foi corrigido para reflectância na base da atmosfera, porém segundo nossa fonte de pesquisa o dado Landsat 8 Level 2 com os algoritmos de refletância de superfície LEDAPS e LaSRC que corrigem os efeitos de dispersão e absorção temporal, espacial e espectral de gases atmosféricos, aerossóis e vapor d'água, que são necessários para caracterizar de forma confiável a superfície terrestre da Terra.

A princípio o que se extrai é que o EVI seria incompatível com os algoritmos de refletância de superfície LEDAPS e LaSRC. Porém é preciso explorar melhor essa hipótese. Seguirei reproduzindo com o referido erro, como estamos explorando algumas outras hipóteses ao passo que exerço a relato será possível, em um momento futuro, entender melhor e propor soluções.

### Salvando o arquivo dois 

Assim como fizemos anteriormente, vamos salvar os arquivos .tif que contém os índices.

```R
writeRaster(x = indices, filename = "R/GDS/saida/area_indices.tif")
names_indices <- names(indices)
write.csv(x = names_indices, file = "R/GDS/saida/area_indices.csv")
```

### Amostra Selecionada para Classificação no QGIS

Para quem desejar apenas reproduzir os resultados que se seguem, basta acessar a pasta ou [baixar o arquivo](https://github.com/EloizioHMD/RemoteSensing_MachineLearning_R_QGIS/raw/main/arquivos/mod_classif.zip) das amostras para dar sequência ao processo. No entanto, discorrerei como realizar uma boa amostragem do que se deseja classificar. Para isso será necessário fazer uso do [software QGIS](https://www.qgis.org/pt_BR/site/) para desenvolver um arquivo vetorial contendo amostras classificadas. 

Essa é uma metodologia conhecida por classificação supervisionada, ou seja, o modelo classificador irá a partir de um dado fornecido a ele tentar reproduzir para toda extensão do dado as informações como classificamos. Trata-se, portanto, de um algoritmo de Machine Learning, assim é importante compreender que caso o modelo de amostras contenha erros na forma como você classificou esse erro será propagado no modelo.

Primeiramente, tenha em mente o que se deseja classificar, organize uma lista de 1 a N feições. No nosso caso veremos que selecionamos 6 feições diferentes (água, agricultura, Caatinga Arbórea Densa, Caatinga Herbácea Arbustiva, solo exposto e área urbana). 

O conhecimento do território é muito importante nesse tipo de processo, dados de clima, relevo, vegetação nativa, irão lhe fornecer os insight necessário para escolher as melhores amostras. Tente fazer uma seleção das feições de forma distribuída ao longo da área estudada. Faça a seleção evitando as bordas do objeto classificado. Busque fazer o máximo de seleção, desde que estas sejam acuradas o suficiente para não confundir o modelo.

Sugiro sempre que baixe o arquivo anteriormente destacado para explorarção.

<p align="center">
  <img src="https://github.com/EloizioHMD/RemoteSensing_MachineLearning_R_QGIS/raw/main/arquivos/img/Captura%20de%20tela%20de%202021-07-06%2018-04-16.jpeg">
</p>

### Carregando arquivo de classificacao gerado no QGIS

Para carregar os dados produzidos no QGIS devem estar no formato shapefile e para carregar apenas o arquivo que contem a geometria, que é o arquivo .shp, usando o `readOGR`.
```R
amostra_classif <- readOGR("R/GDS/vector/mod_classif.shp")
```
É possível visualizar os dados carregados pelo comando `View(data.frame(amostra_classif))`.

## Criando Modelo de Treinamento

Neste ponto construiremos os modelos de treinamento para realizar a classificação. Falo no plural que para testarmos a hipótese se os indicies espectrais contribuem ou não com a classificação supervisionada faremos dois modelos, um apenas com as bandas e outro com bandas e índices.

### Dissolvendo poligonos para valores unicos

Primeiramente é preciso unir todos os valores únicos para feições unicas.

```R
classif_dslv <- gUnaryUnion(spgeom = amostra_classif, id = amostra_classif$classe)
```

Se passarmos o comando `classif_dslv` para verificar veremos que o objeto passou de 92 features restaram apenas 6.

### Extrair atributos das classes

Com a informação anteriormente estruturada, vamos ao avanço dele e cruzar as feições classificadas com o raster (neste momento apenas das bandas).

```R
atributos_band <- raster::extract(x = L8_B1B7, y = classif_dslv)
```

Esse processamento apresenta uma certa demora, variará conforme a capacidade de CPU da máquina onde está sendo processado. 

### Criando o datafrane para cada classe 

Durante a criação das classes selecionei as feições que presentavam do seguinte modo:

| valor | classe      | descrição                                                    |
| ----- | ----------- | ------------------------------------------------------------ |
| 1     | agua        | Corpos hídricos                                              |
| 2     | agricultura | Agricultura irrigada                                         |
| 3     | CAD         | Caatinga Arbórea Densa (CAD): Engloba a vegetação arbórea densa, de porte mais elevado. Nas regiões de serra observa-se uma vegetação com característica mais exuberante, onde as condições climáticas fornecem maior vigor na vegetação. Também está presente nas regiões do interior mais planas e mais secas apresentando uma leve diferença de tonalidade; |
| 4     | CHA         | Caatinga Herbácea Arbustiva (CHA): Segundo Fernades e Bezerra (1990) esta vegetação é do tipo xerófila surgindo em áreas com características de semiaridez. Engloba a vegetação herbácea arbustiva (porte baixo a médio) aberta à densa; |
| 5     | solo        | Solo exposto                                                 |
| 6     | urbano      | Malha urbana                                                 |

Assim, criaremos um dataframe para cada classe observada por meio do código:

```R
agua <- data.frame(Classe = "Agua", atributos_band[[1]])
agricultura <- data.frame(Classe = "Agricultura", atributos_band[[2]])
CAD <- data.frame(Classe = "Caatinga Arborea Densa", atributos_band[[3]])
CHA <- data.frame(Classe = "Caatinga Herbacea Arbustiva", atributos_band[[4]])
solo <- data.frame(Classe = "Solo exposto", atributos_band[[5]])
urbano <- data.frame(Classe = "Urbano", atributos_band[[6]])
```

Posteriormente combinamos os dataframes com:

```R
classif_band <- rbind(agua, agricultura, CAD, CHA, solo, urbano)
```

Ao final desse processo o objeto  `classif_band` deve possuir 80.668 valores classificados como modelo da amostras.

De igualmodo, mudando o que tem que mudar, fizemos uma outro modelo incluindo os índices espectrais por meio do seguinte código:

```R
# Extrair atributos das classes com L8_INDEX
atributos_index <- raster::extract(x = L8_INDEX, y = classif_dslv)

# criando df para cada classe
agua_i <- data.frame(Classe = "Agua", atributos_index[[1]])
agricultura_i <- data.frame(Classe = "Agricultura", atributos_index[[2]])
CAD_i <- data.frame(Classe = "Caatinga Arborea Densa", atributos_index[[3]])
CHA_i <- data.frame(Classe = "Caatinga Herbacea Arbustiva", atributos_index[[4]])
solo_i <- data.frame(Classe = "Solo exposto", atributos_index[[5]])
urbano_i <- data.frame(Classe = "Urbano", atributos_index[[6]])

# Combinando os df
classif_index <- rbind(agua_i, agricultura_i, CAD_i, CHA_i, solo_i, urbano_i)
```

Este segundo modelo demora mais que o anterior, mas ao final o objeto `classif_index` deve possuir os mesmos 80.668 valores classificados.

## Visualizações dos dados para  realização de análise exploratória

### Gráfico do comportamente das classes espectrais da amostra das bandas

Primeiramente vamos calcular a média para cada uma das classes:

```R
agrupado_band <- group_by(classif_band, Classe)
media_refband <- summarise_each(agrupado_band, mean)
```

Em seguida realizamos a sua transposta:

```R
refband <- t(media_refband[,1:7])
```

Para melhorar a visualização das feições classificadas e organizar conforme os comprimentos de onda críamos dois objetos:

```R
cores <- c("green", "blue", "yellow", "orange", "brown", "pink")
wavelength <- c(430, 450, 530, 640, 850, 1570, 2110)
```

Assim, para efetivamente construirmos o gráfico usaremos o matplot usando os objetos anteriormente criados no seguinte código:

```R
par(mar = c(5, 5, 4, 16), xpd = TRUE)
matplot(x = wavelength, y = refband, type = 'l', lwd = 2, lty = 1,
        xlab = "Comprimento de Onda (nm)", ylab = "Reflectancia",
        col = cores, ylim = c(0, 30000))
legend("topright", inset = c(- 0.6, 0), legend = media_refband$Classe, lty = 1, col = cores, ncol = 1, lwd = 2)
```

<p align="center">
  <img src="https://github.com/EloizioHMD/RemoteSensing_MachineLearning_R_QGIS/raw/main/arquivos/img/matplot_band.png">
</p>

A área de estudo está em uma região do semiárido, com um baixo índice pluviométrico de apenas 483 milímetros por ano (mm/ano), vegetação nativa possuem características muito diversa daquelas decorrentes de atividade econômica por meio da agricultura irrigada. Os alvos possuem resposta espectral diversas uns dos outros, o que aponta para um bom resultado na classificação que será realizada.

### Gráficos boxplot para compreender as Classes no Índices Espectrais

Primeiramente precisaremos usar uma função `melt()` do rshape2 para executar uma fusão que converte um objeto em um quadro de dados fundido.

```R
classif_melt <- melt(classif_index)
```

Feito isso, poderemos plotar um gráfico no ggplot do tipo boxplot. Segue o código para a plotagem:

```R
ggplot(data = classif_melt, aes(Classe, value, fill = Classe)) + 
  geom_boxplot() + 
  facet_wrap(~variable, scales = 'free') + 
  theme(panel.grid.major = element_line(colour = "#d3d3d3"),
        panel.grid.minor = element_blank(),
        panel.border = element_blank(),
        panel.background = element_blank(),
        text=element_text(family = "Tahoma"),
        axis.title = element_text(face="bold", size = 10),
        axis.text.x = element_text(colour="white", size = 0),
        axis.text.y = element_text(colour="black", size = 10),
        axis.line = element_line(size=1, colour = "black")) +
  theme(plot.margin = unit(c(1,1,1,1), "lines"))
```

<p align="center">
  <img src="https://github.com/EloizioHMD/RemoteSensing_MachineLearning_R_QGIS/raw/main/arquivos/img/plot_zoom_png">
</p>

O boxplot nos permite avaliar o desempenho dos índices junto as bandas. Como já havíamos percebido o EVI apresentou algum erro. Os demais índices de vegetação tiveram comportamento similar, NDWI destacou uma forte diferença para as massas d’água.

Ainda não completamos o processo, mas aparentemente teremos um resultado muito próximo ao outro. Outra revelação interessante talvez seja o de compor com índices que destacam elementos diferentes na paisagem para facilitar a separação dos alvos.

...つづく

### Referências:

> <a name="love">LOVELACE, Robin; NOWOSAD, Jakub; MUENCHOW, Jannes</a>. [Geocomputation with R](https://geocompr.robinlovelace.net/). CRC Press, 2019.<br>

> <a name="dhar">DHAR, Vasant</a>. [Data science and prediction. Communications of the ACM](https://dl.acm.org/doi/abs/10.1145/2500499), v. 56, n. 12, p. 64-73, 2013.<br>

> <a name="tansley">HEY, Tony; TANSLEY, Stewart; TOLLE, Kristin.</a> [O Quarto paradigma: descobertas científicas na era da eScience](https://books.google.com.br/books?hl=pt-BR&lr=&id=bVm_4X4Wm-UC&oi=fnd&pg=PT4&dq=o+quarto+paradigma&ots=Pzplg6bdlr&sig=RtQy80y4bLEvU60NbgLOVckO4AY#v=onepage&q=o%20quarto%20paradigma&f=false). Oficina de Textos, 2011.<br>

> <a name="wiki">PETROLINA. In: WIKIPÉDIA</a>, a enciclopédia livre. Flórida: Wikimedia Foundation, 2021. Disponível em: <https://pt.wikipedia.org/w/index.php?title=Petrolina&oldid=61213019>. Acesso em: 22 mai. 2021.<br>

> <a name="masek">MASEK, Jeffrey G. et al</a>. Landsat 9: Empowering open science and applications through continuity. Remote Sensing of Environment, v. 248, p. 111968, 2020.<br>

> <a name="usgs">United States Geological Survey, USGS</a>. Landsat Collection 2 Level-2 Science Products, 2021. Disponível em: <https://www.usgs.gov/core-science-systems/nli/landsat/landsat-collection-2-level-2-science-products>. Acesso em: 02 jun. 2021.<br>

> <a name="mene">MENESES, Paulo Roberto; ALMEIDA, T. de.</a> Introdução ao processamento de imagens de sensoriamento remoto. Universidade de Brasília, Brasília, 2012.

> <a name="band">ESRI. Band Arithmetic function—ArcGIS Pro | Documentation. Disponível em: <https://pro.arcgis.com/en/pro-app/latest/help/analysis/raster-functions/band-arithmetic-function.htm>. Acesso em: <4 de junho de 2021>.

## Sobre mim
Oi, eu sou o Eloízio, formado em Direito e Engenharia Ambiental e Sanitária, Especialista em Geociências e Geotecnologias, Nerd das antigas e agora exploRando o R. [Para mais informações me siga nas redes](https://linktr.ee/eloiziodantas).
