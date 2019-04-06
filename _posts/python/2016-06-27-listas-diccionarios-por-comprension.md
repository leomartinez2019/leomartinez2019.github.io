---
layout: post
description: 'Breve recuento de las listas por comprensión'
title: Listas y Diccionarios por Comprensión
date: 2016-06-27 14:25:28 -0500
categories: python
---

<h1 class="centrar-titulo-blog">Listas y Diccionarios por Comprensión</h1>

Las listas por comprensión son análogas a los conjuntos por comprensión en teoría de conjuntos. Por ejemplo, A es el conjunto de las vocales (por comprensión) o A = {a,e,i,o,u} (por extensión).

En Python las listas por comprensión se expresan por medio de claúsulas for o if, por ejemplo, la lista A = [0,1,2,3,4] se expresa por comprensión como:

{% highlight python %}
A = [x for x in range(5)]
{% endhighlight %}

La primera parte de la lista es una expresión que se va a evaluar de acuerdo a las cláusulas if o for. 
Por ejemplo para obtener la lista de los números enteros múltiplos de 3 menores que 20:

{% highlight python %}
mult_3 = [x for x in range(1,20) if x%3 == 0]
print mult_3
[3, 6, 9, 12, 15, 18]
{% endhighlight %}

Este tipo de listas se pueden usar para resolver el primer problema del 
proyecto Euler(<a href="https://projecteuler.net/" target="_blank">Project Euler</a>):
<hr>
**MULTIPLES OF 3 AND 5.**

**Problem 1.**

If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.
Find the sum of all the multiples of 3 or 5 below 1000.
<hr>

Por ejemplo, los números múltiplos de 5 menores que 20:

{% highlight python %}
[x for x in range(1,20) if x%5 == 0]
[5, 10, 15]
{% endhighlight %}

Los múltiplos de 3 menores que 20:

{% highlight python %}
[x for x in range(1,20) if x%3 == 0]
[3, 6, 9, 12, 15, 18]
{% endhighlight %}

La unión de estas dos listas sería:

{% highlight python %}
[x for x in range(1,20) if x%3 == 0 or x%5==0]
[3, 5, 6, 9, 10, 12, 15, 18]
{% endhighlight %}

Si usamos la misma notación para números menores que 1000:

{% highlight python %}
[x for x in range(1,1000) if x%3 == 0 or x%5==0]
{% endhighlight %}

Para hallar la suma podemos usar la función sum:

{% highlight python %}
sum([x for x in range(1,1000) if x%3 == 0 or x%5==0])
233168
{% endhighlight %}

Para crear **diccionarios por comprensión** es muy conveniente usar la función **zip()** que devuelve tuplas consistentes
del primer elemento de una lista con el primer elemento de la segunda lista, el segundo elemento de una con el
segundo elemento de la otra y así sucesivamente:
{% highlight python %}
valores = [1,2,3]
claves = ['a', 'b', 'c']
zip(claves, valores)
[('a', 1), ('b', 2), ('c', 3)]
{a:b for a,b in zip(claves, valores)}
{'a': 1, 'b': 2, 'c': 3}
{% endhighlight %}

Es posible crear **listas o diccionarios anidados** como en este caso:
{% highlight python %}
datos = [['Luisa', 23, 'Brazil'], ['Tom', 34, 'USA'], ['Jean', 29, 'Francia']]
campos = ['nombre', 'edad', 'origen']
[{clave: valor for clave, valor in zip(campos, lista)} for lista in datos]
[
    {'edad': 23, 'nombre': 'Luisa', 'origen': 'Brazil'},
    {'edad': 34, 'nombre': 'Tom', 'origen': 'USA'},
    {'edad': 29, 'nombre': 'Jean', 'origen': 'Francia'}
]
{% endhighlight %}