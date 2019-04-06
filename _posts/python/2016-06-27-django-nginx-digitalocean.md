---
layout: post
title: "Montando un proyecto django en Ubuntu con nginx"
description: "Una Aplicación en digitalocean"
date: 2016-06-27 8:25:28 -0500
categories: python
---

# Montando un proyecto django en Ubuntu con nginx

<p>
  Digitalocean es un proveedor de sevicios en la nube para desarrolladores de
  software. Un aspecto positivo de Digitalocean es que tiene una excellente
  documentación y tutoriales desde cómo iniciar un servidor hasta cómo instalar
  todo tipo de software. Las otras opciones para despliegue de aplicaciones son 
  <a href="https://www.linode.com" target="_blank">Linode</a>, <a href="https://aws.amazon.com" target="_blank">AWS</a> 
  <a href="https://www.heroku.com" target="_blank">Heroku</a>, entre otras. El presente artículo está basado
  precisamente en un tutorial de Digitalocean que se puede leer 
  <a href="https://www.digitalocean.com/community/tutorials/how-to-set-up-django-with-postgres-nginx-and-gunicorn-on-ubuntu-14-04" target="_blank">aquí</a>.
</p>

<h3 class="subtitulo-blog">Crear droplet e implementar las claves SSH</h3>
<p>
  Lo primero que hay que hacer es crear una cuenta en Digitalocean con datos de
  pago con tarjeta de crédito. Se pueden escoger Droplets de Ubuntu, CentOS,
  Fedora, Debian FreeBSD y CoreOS que cuestan mínimo 5 dólares mensuales.Toma un minuto crear un droplet. Digitalocean envia un correo con la dirección IP y una clave para acceder por primera vez:
{% highlight nano %}
Droplet Name: ubuntu-512mb-nyc3-01
IP Address: 159.34.67.123
Username: root
Password: 80089747ca9ee0c7
{% endhighlight %}
</p>
<p>
  Hay que generar en el computador local las claves ssh. En Windows se puede usar
  la aplicación Cygwin, en Mac y Linux la línea de comandos.
{% highlight vim %}
$ ssh-keygen -t rsa
{% endhighlight %}
</p>
<p>
  Se escoge donde guardarlo y luego se copia el archivo pub y se pega en el campo
  ssh key de Digitalocean. O también, y mejor, se puede usar el comando ssh-copy-id:
{% highlight vim %}
$ ssh-copy-id root@159.34.67.123
{% endhighlight %}
  Luego pide la clave que es la que se había enviado por correo.
{% highlight nano %}
  $ ssh-copy-id root@159.34.67.123 (usar la dirección IP asignada)
  /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
  /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
  root@159.34.67.75's password:

  Number of key(s) added: 1

  Now try logging into the machine, with:   "ssh 'root@159.34.67.123'"
  and check to make sure that only the key(s) you wanted were added.
{% endhighlight %}
</p>
<h3 class="subtitulo-blog">Conectarse al servidor desde el computador local</h3>
<p>
  Se usa ssh y ya no es necesario ingresar la clave:
{% highlight vim %}
$ ssh root@159.34.67.123
{% endhighlight %}
</p>
<p>
  Siempre recomiendan por seguridad crear un usuario con privilegios administrativos y no hacer <i>logging</i> como root.
{% highlight nano %}
  # adduser usuario
  # passwd usuario (agregar contraseña)
  # adduser -aG sudo usuario
{%  endhighlight %}
A continuación hay que salir del servidor y volver a ingresar como el nuevo usuario:
{% highlight nano %}
  # exit
  $ ssh-copy-id usuario@159.34.67.123
  $ ssh usuario@159.34.67.123
{% endhighlight %}
</p>
<h3 class="subtitulo-blog">Instalar nginx, postgres y demás dependencias</h3>
<p>
{% highlight vim %}
$ sudo apt-get install python-pip python-dev postgresql postgresql-contrib nginx
{% endhighlight %}
Hay que crear un nuevo usuario de postgres y la base de datos que se va a usar en django. Ingresamos a postgres:
{% highlight nano %}
sudo su - postgres
psql
CREATE DATABASE proyecto;
CREATE USER usuario WITH PASSWORD 'password';
GRANT ALL PRIVILEGES ON DATABASE proyecto TO usuario;
\q
exit
{% endhighlight %}
</p>
<h3 class="subtitulo-blog">Crear entorno virtual y el proyecto django</h3>
Vamos a crear un entorno virtual con virtualenv, la carpeta del proyecto y luego descargamos los programas que faltan con <i>python pip</i>:
{% highlight vim %}
$ sudo pip install virtualenv
$ mkdir ~/proyecto
$ cd ~/proyecto
$ virtualenv mientorno
$ source mientorno/bin/activate
$ pip install django gunicorn psycopg2
{% endhighlight %}
Gunicorn (green unicorn) es un servidor wsgi que funciona bien en ambientes linux y psycopg2 permite la comunicación entre python y postgresql.
Lo que sigue es crear el proyecto django en directorio actual y modificar el archivo settings:
{% highlight vim %}
$ djando-admin startproject proyecto .
$ cd proyecto
$ vim settings.py
{% endhighlight %}
Modificamos los valores de la base de datos y la ubicación de los archivos estáticos:
{% highlight vim %}
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'proyecto',
        'USER': 'usuario',
        'PASSWORD': 'HTGsaMwsg54',
        'HOST': 'localhost',
        'PORT': '',
    }
}

STATIC_ROOT = os.path.join(BASE_DIR, "static/")
{% endhighlight %}

Luego hacemos las migraciones de la base de datos, creamos un superusuario y agrupamos los archivos estáticos en un solo lugar:
{% highlight vim %}
$ cd ~/proyecto
$ python manage.py makemigrations
$ python manage.py migrate
$ pythom manage.py createsuperuser
$ python manage.py collectstatic
{% endhighlight %}

<h3 class="subtitulo-blog">Configurar Gunicorn y Nginx</h3>
La última parte del despliegue django es configurar gunicorn para que mantenga la aplicación corriendo. Modificamos el archivo /etc/init/gunicorn.conf para que quede de la siguiente forma:
{% highlight vim %}
description "Gunicorn application server handling proyecto"

start on runlevel [2345]
stop on runlevel [!2345]

respawn
setuid usuario
setgid www-data
chdir /home/usuario/proyecto

exec mienv/bin/gunicorn --workers 3 --bind unix:/home/usuario/proyecto/proyecto.sock proyecto.wsgi:application
{% endhighlight%}
Iniciamos gunicorn:
{% highlight vim %}
$ sudo service gunicorn start
{% endhighlight %}
Finalmente se crea el archivo /etc/nginx/sites-available/proyecto:

{% highlight vim %}
server {
   listen 80;
   server_name .example.com;

   location = /favicon.ico { access_log off; log_not_found off; }
   location /static/ {
       root /home/usuario/proyecto;
   }

   location / {
       include proxy_params;
       proxy_pass http://unix:/home/usuario/proyecto/proyecto.sock;
   }
}
{% endhighlight %}
Creamos un enlace simbólico y revisamos que la configuración de nginx es correcta:
{% highlight vim %}
$ sudo ln -s /etc/nginx/sites-available/proyecto /etc/nginx/sites-enabled
$ sudo nginx -t
{% endhighlight %}
Iniciamos nginx y ya podemos ir a la dirección IP a revisar el sitio django.
{% highlight vim %}
$ sudo service nginx start
$ curl 159.34.67.123
{% endhighlight %}
