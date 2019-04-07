---
layout: post
title:  'Comenzando con Docker en CentOS'
description: 'Docker básico en CentOS'
date:   2019-04-07 00:00:00 -0500
categories: linux
---

Docker facilita el manejo de contenedores. Los contenedores en Linux permiten que las aplicaciones se ejecuten
independiente del sistema donde están alojados. Un desarrollador puede empaquetar una aplicación junto con las dependencias
y librerías. Esto permite simplicar la configuración y la depuración de código tanto en producción como en desarrollo.

Estas son las instrucciones para instalar docker en un servidor centOS del tipo Digital Ocean o Linode.
Para instalar docker en CentOS primero revisamos que no haya versiones previas de Docker:

{% highlight bash %}
  sudo yum remove -y docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
{% endhighlight %}

Luego se instalan las herramientas necesarias para comenzar con Docker:

{% highlight bash %}
  sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
{% endhighlight %}

A continuación se agrega el código estable:

{% highlight bash %}
  sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
{% endhighlight %}

Ahora se puede instalar docker:

{% highlight bash %}
  sudo yum -y install docker-ce
{% endhighlight %}

En este momento se puede iniciar y activar docker:

{% highlight bash %}
  sudo systemctl start docker && sudo systemctl enable docker
{% endhighlight %}

Podemos comprobar que docker está instalado descargando y ejecutando hello-world:

{% highlight bash %}
  sudo docker run hello-world
{% endhighlight %}

Para que un usuario diferente de *root* pueda ejecutar docker hay que agregarlo al grupo:

{% highlight bash %}
  sudo usermod -aG docker usuario
{% endhighlight %}

Para que el cambio se haga efectivo el usuario debe salir e ingresar de nuevo al servidor.

### Comandos básicos

Mostrar las imágenes instaladas:

{% highlight bash %}
  docker image ls
{% endhighlight %}

Traer la imagen de nginx de Docker Hub:

{% highlight bash %}
  docker image pull nginx
{% endhighlight %}

Ver detalles de la imagen:

{% highlight bash %}
  docker image inspect nginx
{% endhighlight %}

Ver los contenedores instalados:

{% highlight bash %}
  docker container ls
{% endhighlight %}

Ejecutar el contenedor nginx:

{% highlight bash %}
  docker container run nginx
{% endhighlight %}

Ver los contenedores en ejecución:

{% highlight bash %}
  docker container ps
{% endhighlight %}

Generar imagen con contenido html estático con nginx ejecutándose en segundo plano:

{% highlight bash %}
  docker run --name mynginx -p 80:80 -d -v ~/docker-nginx/html:/usr/share/nginx/html nginx
{% endhighlight %}

Ejecutar comandos de forma interactiva usando el bash del contenedor.
Entramos en el prompt del contenedor:

{% highlight bash %}
  docker container exec -it 74124f68c770 /bin/bash
  root@74124f68c770:/#
  root@74124f68c770:/# ls
  root@74124f68c770:/# exit
{% endhighlight %}

Pausar el contenedor:

{% highlight bash %}
  docker container pause 74124f68c770
{% endhighlight %}

Borrar un contenedor que está en ejecución:

{% highlight bash %}
  docker container rm -f 74124f68c770
{% endhighlight %}

Borrar todos los contenedores que no están en ejecución:

{% highlight bash %}
  docker container prune
{% endhighlight %}
