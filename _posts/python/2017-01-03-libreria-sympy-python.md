---
layout: post
description: 'Breve mirada a la librería Sympy de Python'
title: Breve resumen de la librería de matemática simbólica Sympy
date: 2017-01-03 05:25:28 -0500
categories: python
mathjax: true
---

<h1 class="centrar-titulo-blog">Breve resumen de la librería de matemática simbólica Sympy</h1>
<p>
    Con Sympy se pueden realizar operaciones matemáticas simbólicas de álgebra, cálculo, algebra lineal y ecuaciones
    diferenciales entre otras. Sympy viene por defecto en la
    distribución de Python de <a href="https://www.continuum.io/" target="_blank">Anaconda</a>.
    Aquí un ejemplo de un cálculo con Sympy: $$  x^{2} - 24 = 0  $$
    
    <pre>
        >>> from sympy import *
        >>> x, y = symbols('x y')
        >>> solve(x**2 - 24, x)
        [-2*sqrt(6), 2*sqrt(6)]</pre>
    La función <i>init_printing()</i> permite visualizar mejor los resultados:
    <pre>
        >>> init_printing()
        >>> solve(x**2 -24, x)</pre>
    Y el resultado (aquí uso MathJax para una mejor visualización):
    $$\left [ - 2 \sqrt{6}, \quad 2 \sqrt{6}\right ]$$
    Otro ejemplo esta vez con integrales:
    <pre>
        Integral(cos(x)*exp(x), x)</pre>
    $$ \int e^{x} \cos{\left (x \right )}\, dx $$
    Se puede obtener los símbolos en Latex con la función <i>print</i>:
    <pre>
        >>> print(latex(acos(0)))
        \frac{\pi}{2}</pre>
    Y representado más elegantemente:
    $$ \frac{\pi}{2} $$
</p>
<p>
    Sympy tiene una página interactiva (<a href="http://live.sympy.org/" target="_blank">Online shell</a>) para practicar los comandos
    de matemática simbólica.
</p>
