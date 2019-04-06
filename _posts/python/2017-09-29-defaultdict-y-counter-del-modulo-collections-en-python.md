---
layout: post
title:  'Defaultdict y Counter del Módulo Collections en Python'
description: 'Contenedores alternativos en Python'
date:   2017-09-29 00:23:43 -0500
categories: python
---

Los tipos de datos en este módulo son contenedores y presentan una alternativa a los diccionarios, listas, tuplas y conjuntos.

## defaultdict
Es una subclase de la clase *dict*. Permite expandir un diccionario sin provocar errores al agregar nuevas claves. Como default_factory se pueden incluir listas (*list*), conjuntos (*set*) y enteros (*int*).
Es común utilizar **defaultdict** para acumular datos para al final convertirlo en un diccionario normal.

{% highlight python %}
# Crear un diccionario de clave - secuencia:
>>> paises = dict()
>>> paises['Chile'] = []
>>> paises['Chile'].append('Santiago')
>>> paises['Chile'].append('Valparaiso')
>>> datos['Ecuador'] = []
>>> datos['Ecuador'].append('Cuenca')
>>> paises['Chile']
['Santiago', 'Valparaiso']
>>> paises['Ecuador']
['Cuenca']

# La siguiente clave no está presente y provoca un error
>>> paises['Brasil']
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'Brasil'

# Con defaultdict evitamos la inicialización manual y el KeyError
>>> from collections import defaultdict
>>> datos = defaultdict(list)
>>> datos['Chile'].append('Santiago')
>>> datos['Chile'].append('Valparaiso')
>>> datos['Ecuador'].append('Cuenca')
>>> datos
defaultdict(<class 'list'>, {'Chile': ['Santiago', 'Valparaiso'], 'Ecuador': ['Cuenca']})
>>> datos['Chile']
['Santiago', 'Valparaiso']
>>> datos['Ecuador']
['Cuenca']

# La siguiente clave no aparece pero no hay error
# en cambio devuelve una lista vacía
>>> datos['Brasil']
[]

# Podemos convertirlo finalmente a diccionario
>>> dict(datos)
{'Ecuador': ['Cuenca'], 'Chile': ['Santiago', 'Valparaiso'], 'Brasil': []}
{% endhighlight %}

## Counter
Es muy útil para conteos eficientes y es equivalente a un multiconjunto. Según [Wikipedia](https://es.wikipedia.org/wiki/Multiconjunto):
<blockquote>
En matemáticas un multiconjunto (también llamado bolsa o bag) difiere de un conjunto en que cada miembro del mismo tiene asociada una multiplicidad (un número natural), indicando cuántas veces el elemento es miembro del conjunto. Por ejemplo, en el multiconjunto { a, a, b, b, b, c }, las multiplicidades de los miembros a, b, y c son 2, 3, y 1, respectivamente.
</blockquote>

{% highlight python %}
# Ejemplo con una cadena de texto:
>>> from collections import Counter
>>> palabra = "efervescentemente" # la vocal 'e' aparece varias veces
>>> c = Counter(palabra)
>>> c
Counter({'e': 7, 't': 2, 'n': 2, 'r': 1, 's': 1, 'c': 1, 'f': 1, 'v': 1, 'm': 1})
# Las tres letras más comunes:
>>> c.most_common(3)
[('e', 7), ('t', 2), ('n', 2)]
# Las tres menos comunes:
>>> c.most_common()[:-4:-1]
[('m', 1), ('v', 1), ('f', 1)]

# Ejemplo con una lista:
>>> lista = ['red', 'blue', 'red', 'black', 'blue', 'blue', 'red', 'red']
>>> conteo = Counter(lista)
>>> conteo
Counter({'red': 4, 'blue': 3, 'black': 1})

# Ejemplo con un diccionario:
>>> d = Counter({'a favor': 5, 'en contra': 3})
>>> d
Counter({'a favor': 5, 'en contra': 3})
>>> e = Counter({'a favor': 2, 'en contra': 7})

# Podemos sumar los dos contadores:
>>> total = d + e
>>> total
Counter({'en contra': 10, 'a favor': 7})

# No hay error si una clave no está presente. El conteo es cero:
>>> d['neutral']
0

# Podemos calcular el conteo en general:
>>> sum(total.values())
17
{% endhighlight %}