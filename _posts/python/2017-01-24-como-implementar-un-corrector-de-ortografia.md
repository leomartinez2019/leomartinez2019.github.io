---
layout: post
description: 'El corrector de ortografía de Peter Norvig en español'
title: Cómo implementar un corrector de ortografía
date: 2017-01-24 12:25:28 -0500
categories: python
---

<h1 class="centrar-titulo-blog">Cómo implementar un corrector de ortografía</h1>
<p>
    En el año 2007 Peter Norvig, director de investigaciones en Google, escribió un artículo en su blog 
    llamado <a href="http://norvig.com/spell-correct.html" target="_blank">"How to Write a 
    Spelling Corrector"</a>. He tratado de adaptarlo en español usando el corpus
    de las palabras y sus frecuencias en español extraído de la página de la Real Academia de la 
    Lengua. El corpus de referencia del español actual (CREA) se puede consultar 
    <a href="http://corpus.rae.es/lfrecuencias.html" target="_blank">aquí</a>.
</p>
<p>
    La corrección de ortografía es un área de investigación del procesamiento
    natural del lenguaje (PNL) que usa la lingüística, la informática y la 
    inteligencia artificial para que las máquinas decodifiquen el lenguaje humano.
    Los errores comunes en español pueden ser: omisión, transposición, adición
    o remplazo de letras. Un ejemplo de transposición pude ser "cuidad" por "ciudad",
    de omisión "sempre" por "siempre", de adición "enrroque" por "enroque". Hay que
    incluir el mal uso de las tildes en español que es un problema que no se ve
    en el idioma inglés.
</p>
El CREA contiene 737799 palabras con sus respectivas
frecuencias absolutas y normalizadas.
{% highlight vim %}
Orden	Frec.absoluta 	 Frec.normalizada 
     1.	de	9,999,518 	 65545.55 
     2.	la	6,277,560 	 41148.59 
     3.	que 	4,681,839 	 30688.85 
     4.	el	4,569,652 	 29953.48 
     5.	en	4,234,281 	 27755.16 
     6.	y	4,180,279 	 27401.19 
     7.	a	3,260,939 	 21375.03 
     8.	los	2,618,657 	 17164.95 
     9.	se	2,022,514 	 13257.31 
    10.	del	1,857,225 	 12173.87 
{% endhighlight %}
Luego de determinar que la suma total de las frecuencias es 134404155 y de modificar
el archivo anterior a la siguiente forma:
{% highlight vim %}
de 9999518
la 6277560
que 4681839
el 4569652
en 4234281
y 4180279
a 3260939
los 2618657
se 2022514
del 1857225
{% endhighlight %}

Si llamamos a este nuevo archivo **frecuencias.txt**, podemos crear un 
diccionario de frecuencias y combinarlo con el código que usa Peter Norvig:
{% highlight python %}

N = 134404155.0 # suma de todas  las frecuencias absolutas
PALABRAS = dict()

with open('frecuencias.txt', 'r') as datafile:
    for line in datafile:
        valores = line.strip('\n').split()
        PALABRAS[valores[0]] = int(valores[1])

def P(palabra, N=sum(PALABRAS.values())): 
    "Probabilidad de `palabra`."
    return PALABRAS[palabra] / N

def correccion(palabra): 
    "Corrección más probable de una palabra."
    return max(candidatos(palabra), key=P)

def candidatos(palabra): 
    "Genera posibles correcciones para una palabra."
    return (conocidas([palabra]) or conocidas(edicion1(palabra)) or conocidas(edicion2(palabra)) or [palabra])

def conocidas(palabras): 
    "El subconjunto de `palabras` que aparecen en el diccionario de PALABRAS."
    return set(w for w in palabras if w in PALABRAS)

def edicion1(palabra):
    "Todas las ediciones que están a una edición de `palabra`."
    letras    = 'abcdefghijklmnopqrstuvwxyzáéíóúüñ'
    divisiones     = [(palabra[:i], palabra[i:])    for i in range(len(palabra) + 1)]
    omisiones    = [L + R[1:]               for L, R in divisiones if R]
    transposiciones = [L + R[1] + R[0] + R[2:] for L, R in divisiones if len(R)>1]
    remplazos   = [L + c + R[1:]           for L, R in divisiones if R for c in letras]
    inserciones    = [L + c + R               for L, R in divisiones for c in letras]
    return set(omisiones + transposiciones + remplazos + inserciones)

def edicion2(palabra): 
    "Todas las ediciones que están a dos ediciones de `palabra`."
    return (e2 for e1 in edicion1(palabra) for e2 in edicion1(e1))
{% endhighlight %}

Se agregaron las letras adicionales **áéíóúüñ** usadas en español.

Estos son ejemplos del resultado de la función correccion:
{% highlight python %}
>>> correccion('domigno')
'domingo'
>>> correccion('luness')
'lunes'
>>> correccion('miércole')
'miércole'
>>> correccion('juves')
'jueves'
>>> correccion('vieernek')
'viernes'
{% endhighlight %}

Como se puede ver el corrector no corrige bien la tercera palabra por lo que
habría que revisar el código para esos casos.