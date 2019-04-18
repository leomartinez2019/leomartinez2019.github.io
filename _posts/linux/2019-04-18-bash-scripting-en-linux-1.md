---
layout: post
title:  "Bash Scripting en Linux (1)"
description: ""
date:   2019-04-17 00:05:28 -0500
categories: linux
---

## ¿Qué es Bash?

Bash es el acrónimo de "bourne-again shell" y es un shell, es decir, un macroprocesador de comandos 
de Linux (Unix). Macroprocesador significa que permite crear expresiones complejas que aumentan
la funcionalidad de la línea de comandos.


## ¿Qué es un shell script?

Un shell script es un archivo que indica el shell a usar, en este caso bash (**#!/bin/bash**) y 
los comandos que se van a ejecutar

Ejemplo de shell script:

{% highlight PowerShell %}
script1.sh
#!/bin/bash

# Copiar lista de archivos de la carpeta del usuario
# y copiarlos en un archivo de texto
ls $HOME >> archivo.txt

{% endhighlight %}

Para ejecutar el archivo se cambian los permisos
{% highlight PowerShell %}
chmod u+x script1.sh
./script.sh
{% endhighlight %}

Si se utiliza bash o un punto . no es necesario cambiar los permisos:
{% highlight PowerShell %}
bash script.sh

. script1.sh
{% endhighlight %}

## Cálculos aritméticos en bash

Para operaciones aritméticas se pueden usar los comandos **expr** y **$(( ))**:
{% highlight PowerShell %}
# Suma:
echo `expr 5 + 2`
echo $((5 + 2))

# Multiplicación:
echo `expr 15 \* 3` # Hay que escapar el símbolo *
echo $((15 * 3))

# Potenciación:
echo $((2 ** 3))

# División:
echo `expr 10 / 5`
echo $((10 / 5))

# Módulo:
echo `expr 10 % 5`
echo $((10 % 5))
{% endhighlight %}

## Leer el input del usuario con **read**:
{% highlight PowerShell %}
#!/bin/bash

# Sumar dos valores sumistrados por el usuario:
echo "Ingresa dos números: "
read x y
suma = $((x + y))

echo "La suma es: $suma"
{% endhighlight %}


## Variables de entorno y bash

En los scripts bash se pueden manipular, crear e invocar variables de entorno:
{% highlight PowerShell %}
#!/bin/bash

# Crear una variable:
export MYVAR="hello"

# Invocar una variable:
echo $HOME
echo $SHELL
echo $LOGNAME

# Usar sustitución de comandos (acento grave):
export HOY=`date`
echo "Hoy es $HOY"
{% endhighlight %}

## Leer archivos

Dado el siguiente archivo de texto:
{% highlight PowerShell %}
# Archivo paises.txt
Chile Santiago
Perú Lima
Ecuador Quito
Rusia Moscú
Francia Paris
{% endhighlight %}

Para leer el archivo línea por línea usamos el siguiente script:

{% highlight PowerShell %}
#!/bin/bash
ARCHIVO=paises.txt

while read -r PAIS CIUDAD;do
  echo "$CIUDAD es la capital de $PAIS"
done < $ARCHIVO
{% endhighlight %}

Y el resultado de la ejecución será:

{% highlight PowerShell %}
Santiago es la capital de Chile
Lima es la capital de Perú
Quito es la capital de Ecuador
Moscú es la capital de Rusia
Paris es la capital de Francia
{% endhighlight %}
