---
layout: post
title:  'Módulo Collections: Deque y ChainMap'
description: 'Más del módulo collections'
date:   2018-02-21 19:52:00 -0500
categories: python
---


## Deque

Deque o *double-ended queue* es un contenedor tipo lista optimizado para agregar
o eliminar elementos. Algunos de los métodos implementados en los deques son:

* append: insertar elemento a la derecha
* extend: insertar elementos de un iterador a la derecha
* pop: eliminar y retornar un elemento de la derecha
* appendleft: insertar a la izquierda
* extendleft: extender a la izquierda
* popleft: eliminar de la izquierda
* rotate(n): rotar *n* pasos a la derecha o a la izquierda si *n* es negativo
* reverse: invertir el orden *in-place*

### Una aplicación trivial: el cifrado César
<div class="foto-center" align="center">
  <img src="{{site.baseurl}}/imagenes/CipherDisk.jpg" alt="caesar cipher" style="width: 100%;" />
  <figcaption></figcaption>
</div>

Según la definición en [Wikipedia](https://es.wikipedia.org/wiki/Cifrado_C%C3%A9sar):

> En criptografía, el cifrado César, también conocido como cifrado por desplazamiento, código de César o desplazamiento de César, es una de las técnicas de cifrado más simples y más usadas. Es un tipo de cifrado por sustitución en el que una letra en el texto original es reemplazada por otra letra que se encuentra un número fijo de posiciones más adelante en el alfabeto. Por ejemplo, con un desplazamiento de 3, la A sería sustituida por la D (situada 3 lugares a la derecha de la A), la B sería reemplazada por la E, etc. Este método debe su nombre a Julio César, que lo usaba para comunicarse con sus generales.

Una forma de simular el movimiento manual de una rueda de cifrado César es usando *deque* der la siguiente forma:

{% highlight python %}
from collections import deque
import string

def cifrado_cesar(texto, desplazamiento):
    """ Simula una rueda de cifrado Cesar """
    alfabeto = deque(string.ascii_lowercase)
    texto_cifrado = ''
    for letra in texto:
        # Ubicamos el índice de la letra original
        indice = alfabeto.index(letra)
        # Rotamos el alfabeto
        alfabeto.rotate(despalazamiento)
        # Obtenemos la nueva letra
        nueva_letra = alfabeto[indice]
        # Restauramos el orden original del alfabeto
        alfabeto.rotate(-despalazamiento)
        texto_cifrado += nueva_letra
    return texto_cifrado

{% endhighlight %}

## ChainMap

Permite almacenar múltiples diccionarios en un solo contenedor. Los diccionarios se guardan en una lista y son accesibles con métodos normales de diccionarios.

{% highlight python %}
from collections import ChainMap

d1 = {'uno': 1, 'dos': 2}
d2 = {'tres': 3, 'cuatro': 4}

d = ChainMap(d1, d2)

>>> d['cuatro']
4
>>> d.maps
[{'dos': 2, 'uno': 1}, {'cuatro': 4, 'tres': 3}]

# En caso de claves repetidas se extrae el primero en orden de aparición:

d3 = {'usuario': 'user1', 'correo': 'user1@gmail.com'}
d4 = {'usuario': 'user2', 'correo': 'user2@gmail.com'}

usuarios = ChainMap(d3, d4)
>>> usuarios.get('usuario')
user1

{% endhighlight %}