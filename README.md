# RemoteSensing_MachineLearning_R_QGIS

Projeto de Geo Data Science sobre Classificação Supervisionada de Imagens do Sensoriamento Remoto de Satélites usando algorítimos de Machine Learning e Índices Espectrais.

![RemoteSensing_MachineLearning_R_QGIS](https://i.imgur.com/MZdanGV.png)<br>

Há muito tempo, em uma galáxia muito, muito distante… está escrito no código jedi das geociências, aindas nas primeiras linhas, “todo padawan em geociência deverá relatar por projetos aquilo que aprendeu de um mestre jedi em geociências por meio da forma que melhor comuniquem suas habilidade como cientista. Certamente esse código existe e em algum recanto das estrelas a força me guia neste momento. 

Recentemente, concluí a jornada "O Primeiro passo para se tornar um expert em Processamento de Imagens de Satélite no R", ministrado pelos instrutores Juliana Diniz e Daniel Maciel, na [RadarGeo](http://plataformaradargeo.com.br/curso_processamento_imagem/). Ao concluir o curso os instrutores encorajaram pela realização de uma projeto com os conhecimentos desenvolvidos ao longo do curso.

Essa proposta vem de encontro com os conteúdos lecionados no livro Geocomputation with R direcionado para quem deseja analisar, visualizar e modelar dados geográficos com software de código aberto baseado em R, uma linguagem de programação estatística que possui poderosos recursos de processamento de dados, visualização e geoespacial [(LOVELACE et al., 2019)](#love).

Lovelace agrupa termos em similaridade e tenta delimitar o escopo de uma ciências que surge das geotecnologias e vêm aproveitando toda evolução das Ciências de Dados. Nesse contexto, Geocomputação, Geographic Information Science (GIScience); Geomática; Geoinformática; Ciência da Informação Espacial; Engenharia de Geoinformação; e Geographic Data Science (GDS), são termos intimamente relacionados. Cada termo compartilha uma ênfase em uma abordagem "científica" (implicando reproduzível e falsificável) influenciada pelo GIS, embora suas origens e principais campos de aplicação sejam diferentes, porém as suas sobreposições entre os termos são maiores do que as diferenças entre eles [(LOVELACE et al., 2019)](#love).

Particularmente, gosto mais de Geographic Data Science (GDS), tem  mais a ver com o que irei comunicar em breve, além de estar bem hypado (modinha). Mas vou deixar um disclaimer para você leitor.

Ciência de dados é um campo interdisciplinar que usa métodos científicos, processos, algoritmos e sistemas para extrair conhecimento e insights de dados estruturados e não estruturados, e aplicar conhecimento e insights acionáveis de dados em uma ampla gama de domínios de aplicação [(DHAR,2013)](#dhar).

No entanto, a ciência de dados é diferente da ciência da computação e da ciência da informação, ou de qualquer outra ciência. O vencedor do Prêmio Turing, Jim Gray, imaginou a ciência de dados como um "quarto paradigma" das ciências (empírica, teórica, computacional e agora baseada em dados) e afirmou que "tudo na ciência está mudando por causa do impacto da tecnologia da informação" e do dilúvio de dados [(TANSLEY, 2009)](#tansley).

### Referências:
> <a name="love">LOVELACE, Robin; NOWOSAD, Jakub; MUENCHOW, Jannes</a>. [Geocomputation with R](https://geocompr.robinlovelace.net/). CRC Press, 2019.<br>
> <a name="dhar">DHAR, Vasant</a>. [Data science and prediction. Communications of the ACM](https://dl.acm.org/doi/abs/10.1145/2500499), v. 56, n. 12, p. 64-73, 2013.<br>
> <a name="tansley">HEY, Tony; TANSLEY, Stewart; TOLLE, Kristin.</a> [O Quarto paradigma: descobertas científicas na era da eScience](https://books.google.com.br/books?hl=pt-BR&lr=&id=bVm_4X4Wm-UC&oi=fnd&pg=PT4&dq=o+quarto+paradigma&ots=Pzplg6bdlr&sig=RtQy80y4bLEvU60NbgLOVckO4AY#v=onepage&q=o%20quarto%20paradigma&f=false). Oficina de Textos, 2011.<br>
