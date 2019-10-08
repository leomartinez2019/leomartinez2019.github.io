---
layout: post
title: "Decoradores en Python"
description: "Uso de decoradores en python"
date: 2019-10-07 18:45:00 -0500
categories: python
---

Un decorador es una función permite cambiar el comportamiento de un método sin modificarlo directamente.
En Python todo se puede considerar un objeto por eso esto es posible:

{% highlight python %}
def first(a):
    def second(b):
        return a + b
    return second

>>> f = first(5)
>>> s = f(7)
12
>>> first(3)(5)
8

{% endhighlight %}

Un decorador no es lo mismo que el patrón de diseño decorador como se advierte en el PEP 318 de [Python Software
Foudation](https://www.python.org/dev/peps/pep-0318/):

> On the name 'Decorator'
> There's been a number of complaints about the choice of the name 'decorator' for this feature.
> The major one is that the name is not consistent with its use in the GoF book.
> The name 'decorator' probably owes more to its use in the compiler area -- a syntax tree is walked and annotated.
> It's quite possible that a better name may turn up.

El siguiente decorador calcula el tiempo de ejecucución entre dos llamados a la misma función:

{% highlight python %}
import time

def cronometro(func):
    'calcula el tiempo entre ejecuciones de la funcion decorada'
    def wrapper(*args, **kwargs):
        if wrapper.time:
            tiempo_transcurrido = time.time() - wrapper.time
            print(f"Tiempo transcurrido: {tiempo_transcurrido}")
        wrapper.time = time.time()
        return func(*args, **kwargs)
    wrapper.time = 0
    return wrapper

def suma_v1(a, b):
    return a + b

# Podemos decorar la función asi:
>>> suma_v1 = cronometro(suma_v1)
>>> suma_v1(2, 3)
5

# Otra forma más común: 
@cronometro
def suma_v2(a, b):
    return a + b

>>> suma(5, 3)
8
>>> suma(4,3)
Tiempo transcurrido: 5.103554010391235
7

{% endhighlight %}

## Uso de *wraps* y *lru_cache* de *functools*

Los atributos como `__name__`, `__doc__` y `__module__` de las funciones originales se pierden de modo que al ejecutar:

{% highlight python %}
print(foo.__name__)
{% endhighlight %}

aparece el nombre de la función wrapper. Para evitar esto usamos wraps:

{% highlight python %}
from functools import wraps

def deco(func):
    @wraps(func)
    def wrapper(*args):
        print(func.__name__)
        return func(*args)
    return wrapper

@deco
def hola():
    print("saludos")

>>> hola()
hola
saludos
>>> hola.__name__
'hola'

# Sin wraps aparecería:
>>> hola.__name__
wrapper

{% endhighlight %}

LRU significa *last recently used* y se usa para guardar en la memoria cache los resultados intermedios en las 
llamadas de las funciones recursivas. Por ejemplo, para calcular la secuencia de fibonacci si se se usa el caché,
la velocidad de ejecución mejora drmáticamente:

{% highlight python %}
import time
from functools import lru_cache

def timer(f):
    'decorador para calcular tiempo de ejecución'
    def wrapper(*args):
        t1 = time.time()
        res = f(*args)
        print(f"duracion: {time.time() - t1}")
        return res
    return wrapper

@lru_cache(maxsize=None)
def fibo1(n):
    if n < 2:
        return n
    return fibo(n-1) + fibo(n-2)

def fibo2(n):
    if n < 2:
        return n
    return fibo2(n-1) + fibo2(n-2)

@timer
test_fibo1():
    fibo1(36)

@timer
test_fibo2():
    fibo2(36)

>>> test_fibo1()
duracion: 4.220008850097656e-05

>>> test_fibo2() # Más de 100 mil veces más lento
duracion: 7.274502754211426

{% endhighlight %}

## Clases como decoradores

Para usar una clase como decorador se debe utilizar la función mágica `__call__` que sirve para que un objeto se
comporte como función:

{% highlight python %}
class decorator:
    def __init__(self, func):
        self.func = func

    def __call__(self):
        print(self.func.__name__)
        self.func()

@decorator
def foo()
    print("hello")

{% endhighlight %}
