---
layout: post
title:  "Cómo Instalar pipenv en Ubuntu"
description: ""
date:   2019-04-15 00:05:28 -0500
categories: linux
---

*Pipenv* es una herramienta de gestión dependencias que crea entornos virtuales para proyectos python. 
Fue creada por Kenneth Reitz, el mismo creador del paquete *requests*. Lo que *pipenv* resuelve es la carencia 
de reproducibilidad de los ambientes virtuales de python y del archivo *requirements.txt*. A veces para que un aplicación se comporte igual en desarrollo y en producción hay que especificar
las versiones exactas de los paquetes:

{% highlight bash %}
  flask=0.12.1
{% endhighlight %}

A esto se le llama "fijar" (pin) una dependencia y generalmente el desarrollador lo hace manualmente.

*Pipenv* tiene un *lockfile* llamado Pipfile.lock que asegura una estructura determinista (reproducible) de la aplicación.

Una forma de instalar *pipenv* es a través de [Linuxbrew](https://docs.brew.sh/Homebrew-on-Linux)

{% highlight bash%}
# Instalar Linuxbrew. En la línea de comandos se ejecuta:
sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"

# Luego se instalan las dependencias de Linuxbrew:
sudo apt-get install build-essential

# Se configura Linuxbrew en  ~/.profile:
echo 'eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)' >>~/.profile

# Se agrega Linuxbrew al PATH:
eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)

# Y se instala gcc (opcional):
brew install gcc
{% endhighlight %}


Ahora se puede instalar pipenv:
{% highlight bash %}
brew install pipenv
{% endhighlight %}

Ejemplo de archivo *Pipfile*:

{% highlight bash %}
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true

[dev-packages]

[packages]
flask = "*"

[requires]
python_version = "3.7"
{% endhighlight %}

El archivo *Pipenv.lock* es tipo json y bastante extenso. Este es una parte de ese archivo:
{% highlight bash %}
"requests": {
            "hashes": [
                "sha256:502a824f31acdacb3a35b6690b5fbf0bc41d63a24a45c4004352b0242707598e",
                "sha256:7bf2a778576d825600030a110f3c0e3e8edc51dfaafe1c146e39a2027784957b"
            ],
            "index": "pypi",
            "version": "==2.21.0"
        },
        "urllib3": {
            "hashes": [
                "sha256:61bf29cada3fc2fbefad4fdf059ea4bd1b4a86d2b6d15e1c7c0b582b9752fe39",
                "sha256:de9529817c93f27c8ccbfead6985011db27bd0ddfcdb2d86f3f663385c6a9c22"
            ],
            "version": "==1.24.1"
        },
        "werkzeug": {
            "hashes": [
                "sha256:0a73e8bb2ff2feecfc5d56e6f458f5b99290ef34f565ffb2665801ff7de6af7a",
{% endhighlight %}


## Uso básico de *pipenv*

{% highlight bash %}
# Crear entorno virtual:
pipenv install

# Crear entorno virtual e instalar tres paquetes:
pipenv install flask requests bs4

# Instalar un paquete para desarrollo:
pipenv install pytest --dev

# Eliminar el entorno virtual por completo:
pipenv --rm

# Activar el entorno virtual:
pipenv shell

# Salir del entorno virtual:
exit

# Ver el árbol de dependencias:
pipenv graph

# La salida de pipenv graph es algo como:
Flask==1.0.2
  - click [required: >=5.1, installed: 7.0]
  - itsdangerous [required: >=0.24, installed: 1.1.0]
  - Jinja2 [required: >=2.10, installed: 2.10.1]
  - MarkupSafe [required: >=0.23, installed: 1.1.1]
  - Werkzeug [required: >=0.14, installed: 0.15.2]
requests==2.21.0
  - certifi [required: >=2017.4.17, installed: 2019.3.9]
  - chardet [required: >=3.0.2,<3.1.0, installed: 3.0.4]
  - idna [required: >=2.5,<2.9, installed: 2.8]
  - urllib3 [required: >=1.21.1,<1.25, installed: 1.24.1]

# Desinstalar un paquete:
pipenv uninstall requests

# Ejecutar un archivo python sin crear el entorno virtual:
pipenv run python app.py

# Revisar la seguridad de los paquetes en el entorno
# y las especificaciones 508:
pipenv check

# La salida puede ser algo como:
Checking PEP 508 requirements…
Passed!
Checking installed package safety…
All good!

# Finalmente ver la ruta del entorno virtual:
pipenv --venv

# Se obtiene la ubicación del entorno:
/home/usuario/.local/share/virtualenvs/FlaskProject-l5CTzLIz
{% endhighlight %}
