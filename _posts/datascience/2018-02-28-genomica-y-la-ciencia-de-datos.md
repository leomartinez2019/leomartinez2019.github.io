---
layout: post
title:  'Genómica y la Ciencia de Datos'
description: 'Python y big data en la genómica'
date:   2018-02-28 19:06:00 -0500
categories: datascience
---

<div class="foto-center" align="center">
  <img src="{{site.baseurl}}/imagenes/centraldogma.jpg" alt="el dogma central" />
  <figcaption>El dogma central de la biología molecular</figcaption>
</div>

**Genómica** es la parte de la biología molecular relacionada con la estructura,
función, evolución y análisis de los genes. Con el proyecto del genoma humano 
se han identificado cerca de 20 mil genes. En las dos últimas décadas el costo 
de la secuenciación del genoma ha disminuido de millones de dólares a sólo unos
cuantos miles de dólares. La primera secuenciación tomó años en completarse.
Hoy toma unos cuantos días hacer lo mismo. El resultado de esta mejora en los
costos y eficiencia ha llevado a la producción de toneladas de datos genómicos
que de alguna forma deben interpretarse.

Hay que recordar que el ADN se compone de cuatro tipos de nucleótidos que forman
una cadena de millones de unidades para almacenar información. La información va
en una dirección desde el ADN del núcleo al ARN mensajero y finalmente a las proteinas
que se sintetizan en los ribosomas y que finalmente son la expresión funcional
del ADN. Las cuatro letras del ADN forman tripletas para codificar cada uno de
los veinte aminoácidos. Podemos usar el módulo **itertools** de Python para obtener
estas tripletas. Se muestra en el siguiente código. Se obtienen 4x4x4 = 64 
tripletas:

{% highlight python%}
>>> from itertools import product
# Obtener todas las palabras de tres letras de un alfabeto de cuatro letras:
>>> tripletas = product("ACTG", 3)
>>> tripletas
<itertools.product object at 0x7f739f9ca6c0>

>>> list(tripletas)
[('A', 'A', 'A'), ('A', 'A', 'C'), ('A', 'A', 'T'), ('A', 'A', 'G'), 
('A', 'C', 'A'), ('A', 'C', 'C'), ('A', 'C', 'T'), ('A', 'C', 'G'), 
('A', 'T', 'A'), ('A', 'T', 'C'), ('A', 'T', 'T'), ('A', 'T', 'G'), 
('A', 'G', 'A'), ('A', 'G', 'C'), ('A', 'G', 'T'), ('A', 'G', 'G'), 
('C', 'A', 'A'), ('C', 'A', 'C'), ('C', 'A', 'T'), ('C', 'A', 'G'), 
('C', 'C', 'A'), ('C', 'C', 'C'), ('C', 'C', 'T'), ('C', 'C', 'G'), 
('C', 'T', 'A'), ('C', 'T', 'C'), ('C', 'T', 'T'), ('C', 'T', 'G'), 
('C', 'G', 'A'), ('C', 'G', 'C'), ('C', 'G', 'T'), ('C', 'G', 'G'), 
('T', 'A', 'A'), ('T', 'A', 'C'), ('T', 'A', 'T'), ('T', 'A', 'G'), 
('T', 'C', 'A'), ('T', 'C', 'C'), ('T', 'C', 'T'), ('T', 'C', 'G'), 
('T', 'T', 'A'), ('T', 'T', 'C'), ('T', 'T', 'T'), ('T', 'T', 'G'), 
('T', 'G', 'A'), ('T', 'G', 'C'), ('T', 'G', 'T'), ('T', 'G', 'G'), 
('G', 'A', 'A'), ('G', 'A', 'C'), ('G', 'A', 'T'), ('G', 'A', 'G'), 
('G', 'C', 'A'), ('G', 'C', 'C'), ('G', 'C', 'T'), ('G', 'C', 'G'), 
('G', 'T', 'A'), ('G', 'T', 'C'), ('G', 'T', 'T'), ('G', 'T', 'G'), 
('G', 'G', 'A'), ('G', 'G', 'C'), ('G', 'G', 'T'), ('G', 'G', 'G')]

>>> len(list(product('ACTG', repeat=3)))
64

{% endhighlight %%}


## Herramientas para el análisis de datos genómicos

### Galaxy
Galaxy es un servicio Web que se usa para la genómica y la investigación
médica. Contiene herramientas de análisis y terabytes de datos y permite al
usuario almacenar sus propios datos. Un problema que Galaxy pretende solucionar
es la reproducibilidad en la investigación genómica lo que significa que si un
investigador suministra los datos y las herramientas informáticas de un experimento,
otro investigador debería ser capaz de reproducir los mismos resultados.
[Enlace a Galaxy](https://usegalaxy.org/)

### Biopython
Biopython contiene módulos para ser usados en biología molecular computacional
y bioinfomática. Con esta herramienta se pueden analizar archivos BLAST (Basic 
Local Alignment Search Tool) para hallar similitudes en secuencias de nucleótidos
o aminoácidos y también archivos de texto FASTA ([Ir a Biopython](http://biopython.org/)):. Un ejemplo de archivo FASTA:

{% highlight bash %}
>P01013 GENE X PROTEIN (OVALBUMIN-RELATED)
QIKDLLVSSSTDLDTTLVLVNAIYFKGMWKTAFNAEDTREMPFHVTKQESKPVQMMCMNNSFNVATLPAE
KMKILELPFASGDLSMLVLLPDEVSDLERIEKTINFEKLTEWTNPNTMEKRRVKVYLPQMKIEEKYNLTS
VLMALGMTDLFIPSANLTGISSAESLKISQAVHGAFMELSEDGIEMAGSTGVIEDIKHSPESEQFRADHP
FLFLIKHNPTNTIVYFGRYWSP
{% endhighlight %}

### NCBI
El NCBI (National Center for Biotechnology Information) trata de ayudar a los
investigadores bioinformáticos brindando acceso a bases de datos y software
para el procesamiento de grandes datos y el análisis de moléculas de importancia
biológica. ([Ir a NCBI](https://www.ncbi.nlm.nih.gov/))

### Bioconductor
Bioconductor usa el lenguaje de programación estadístico R para analizar datos
genómicos. Se usa para analizar, procesar y graficar información biológica. Por
ejemplo, se puede investigar la expresión genética por medio del análisis de ARN.
([Ir a Bioconductor](https://www.bioconductor.org/))

## Recursos para aprender genómica estadística
### Canal de Youtube de NCBI:
<iframe width="560" height="315" src="https://www.youtube.com/embed/-uBY0rnkf8U" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


### Especializacion en genómica de Coursera:
[Genómica y Ciencia de Datos](https://es.coursera.org/specializations/genomic-data-science)
<div class="foto-center" align="center">
  <img src="{{site.baseurl}}/imagenes/genomicsCoursera.png" alt="genomics coursera" />
  <figcaption></figcaption>
</div>
