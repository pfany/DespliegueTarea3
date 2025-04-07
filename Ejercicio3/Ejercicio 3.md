---
Author: Epifanía Peralta López
Title: Ejercicio 3
---

# Ejercicio 3 - imagen con Dockerfile - Aplicación web

> Documento que resuelve el ejercicio 3 de la Tarea 3- Despliegue de Aplicaciones Web, realizado por Epifanía Peralta López

[TOC]

> Para la realización de este ejercicio es necesario tener una cuenta en Docker Hub.
>
> Se puede resolver utilizando comandos, o Docker Desktop, o combinando ambos.



**Enunciado:** 

Necesitamos un fichero **Dockerfile** que automatice las siguientes operaciones para crear una imagen que contenga un servidor con un sitio web y un script php. Características de la imagen:

* Usa un contenedor que ejecute una instancia de la imagen `php:7.4-apache` que se llame `ejercicio3` y que sea accesible desde un navegador en el puerto 8000.
* Coloca en el directorio raíz del servicio web (`/var/www/html`) un "sitio web" donde figure tu nombre -el sitio deberá tener al menos un archivo `index.html` sencillo y un archivo `.css`
* Coloca en ese mismo directorio raíz el siguiente script `php`, llámalo `fecha.php`

```php
<?php
	setlocale(LC_TIME, "es_ES.UTF-8");
	$mes_actual = strftime("%B");
	$fecha_actual = date("d/m/Y");
	$hora_actual = date("H:i:s");
	echo "<h1>Información</h1>";
	echo "<p>Hoy es $fecha_actual</p>";
	echo "<p>El mes es: <strong>$mes_actual</strong></p>";
	echo "<p>Hora: $hora_actual</p>";
?>
```

* Ver la salida del script `fecha.php` y de la página `índex.html` en el navegador.
* Una vez creada la imagen, súbela a tu cuenta de Docker Hub:
  * Borra la imagen de tu Docker local.
  * Baja ('pull') de tu cuenta la imagen que acabas de subir.
  * Muestra las imágenes que tienes.
  * Ejecuta un contenedor usando esa imagen.



**Resolución:**

## Bloque de código con el Dockerfile

La imagen `php:7.4-apache` tiene ya instalado el servidor web y PHP.

Además, no es necesario indica el `CMD` ya que por defecto el contenedor creado a partir de esta imagen ejecutará el mismo proceso que la imagen base, es decir, la ejecución del servidor web.

El código es:

```dockerfile
FROM php:7.4-apache
COPY app /var/www/html
```

En `app/` están colgados los archivos `índex.html`, `style.css` y `fecha.php`, de forma que el contenido de ese directorio se copiará al `/var/www/html/`

<img src="./Ejercicio%203.assets/Monosnap%20Dockerfile%20%E2%80%94%20DespliegueTarea3%202025-04-02%2021-00-34(1).png" alt="Monosnap Dockerfile — DespliegueTarea3 2025-04-02 21-00-34(1)" style="zoom: 33%;" />



## Creación de la nueva imagen

La nueva imagen sería:

```bash
docker build -t pfany/newImage .
```

Explicación:

* `docker build`-> inicia la construcción de la imagen.
* `-t pfany/newImage .`-> indica el nombre de la imagen y que se cree en el directorio actual donde está el archivo Dockerfile (en `Ejercicio3`).

Las capturas del proceso:

<img src="./Ejercicio%203.assets/Monosnap%20Ejercicio3%20%E2%80%94%20-zsh%20%E2%80%94%20125%C3%9729%202025-04-02%2022-24-23.png" alt="Monosnap Ejercicio3 — -zsh — 125×29 2025-04-02 22-24-23" style="zoom: 25%;" />

<img src="./Ejercicio%203.assets/Monosnap%20Ejercicio3%20%E2%80%94%20-zsh%20%E2%80%94%20125%C3%9732%202025-04-02%2022-26-56(1)-3625884.png" alt="Monosnap Ejercicio3 — -zsh — 125×32 2025-04-02 22-26-56(1)" style="zoom: 25%;" />

Así pues, el nombre de la imagen es `pfany/new-image`, ya que Docker no permite nombres de imágenes con mayúsculas.

## Creación del contenedor inicial

El contenedor llamado `ejercicio3` será accesible desde un navegador en el puerto 8000, con lo que a continuación de crear la imagen, creamos el contenedor llamado `ejercicio3` utilizando la magen `pfany/newImage`

```bash
docker run -d -p 8000:80 --name ejercicio3 pfany/new-image
```

Al indicar `-p 8000:80` se hace el mapeo de puertos: mapeo el puerto 8000 del host al puerto 80 del contenedor (donde Apache escucha por defecto).

El resultado es el siguiente:

<img src="./Ejercicio%203.assets/Monosnap%20Ejercicio3%20%E2%80%94%20-zsh%20%E2%80%94%20125%C3%9732%202025-04-02%2022-45-42.png" alt="Monosnap Ejercicio3 — -zsh — 125×32 2025-04-02 22-45-42" style="zoom:33%;" />

<img src="./Ejercicio%203.assets/Monosnap%20Ejercicio3%20%E2%80%94%20-zsh%20%E2%80%94%20125%C3%9732%202025-04-02%2022-46-37.png" alt="Monosnap Ejercicio3 — -zsh — 125×32 2025-04-02 22-46-37" style="zoom:33%;" />



## Acceso al navegador con la página `html` y con el script `php`

Accedo al navegador con el contenedor inicial:

<img src="./Ejercicio%203.assets/Monosnap%20The%20funniest%20web%202025-04-02%2022-48-08.png" alt="Monosnap The funniest web 2025-04-02 22-48-08" style="zoom:33%;" />

que corresponde con el `index.html` muy básico creado:

<img src="./Ejercicio%203.assets/Monosnap%20index.html%20%E2%80%94%20DespliegueTarea3%202025-04-02%2022-50-06.png" alt="Monosnap index.html — DespliegueTarea3 2025-04-02 22-50-06" style="zoom: 25%;" />

Nota: en el index.html indico como título 'The funniest web' haciendo un guiño al diminutivo de mi nombre (Fany).

Al clickar en `Script de fecha y hora`, el resultado es:

<img src="./Ejercicio%203.assets/fecha.php%202025-04-02%2022-51-43.png" alt="fecha.php 2025-04-02 22-51-43" style="zoom:33%;" />



## Subir la imagen creada a mi cuenta en Docker Hub

* Accedo a Docker Hub usando el comando `docker login`, aunque como tengo el Docker Desktop abierto, ya me identifica.
* Subo la imagen a Docker Hub:

```bash
docker login
docker push pfany/new-image
```

La captura de la pantalla:

<img src="./Ejercicio%203.assets/Monosnap%20DespliegueTarea3%20%E2%80%94%20-zsh%20%E2%80%94%20125%C3%9744%202025-04-03%2017-02-19(1).png" alt="Monosnap DespliegueTarea3 — -zsh — 125×44 2025-04-03 17-02-19(1)" style="zoom:25%;" />

Y, efectivamente, en Hub repositories ya consta la imagen creada:

<img src="./Ejercicio%203.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-04-03%2017-05-34.png" alt="Monosnap Images - Docker Desktop 2025-04-03 17-05-34" style="zoom:25%;" />



## Borrar la imágen del Docker local

En este caso, como ya estoy en Docker Desktop, para borrar la imagen:

<img src="./Ejercicio%203.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-04-03%2017-07-49(1).png" alt="Monosnap Images - Docker Desktop 2025-04-03 17-07-49(1)" style="zoom:25%;" />

Y confirmo el borrado:

<img src="./Ejercicio%203.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-04-03%2017-09-09.png" alt="Monosnap Images - Docker Desktop 2025-04-03 17-09-09" style="zoom:25%;" />

Pero efectivamente, primero he de borrar el contenedor que contiene esa imagen:

<img src="./Ejercicio%203.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-04-03%2017-10-37.png" alt="Monosnap Images - Docker Desktop 2025-04-03 17-10-37" style="zoom:25%;" />

En Docker Desktop, muestro los containers:

<img src="./Ejercicio%203.assets/Monosnap%20Containers%20-%20Docker%20Desktop%202025-04-03%2017-11-59(1).png" alt="Monosnap Containers - Docker Desktop 2025-04-03 17-11-59(1)" style="zoom:25%;" />

Después de parar el contenedor, ya se puede borrar y, posteriormente, borrar la imagen.

Al mostrar las imágenes, ya no figura `pfany/new-image`:

<img src="./Ejercicio%203.assets/Monosnap%20DespliegueTarea3%20%E2%80%94%20-zsh%20%E2%80%94%20125%C3%9744%202025-04-03%2017-14-19.png" alt="Monosnap DespliegueTarea3 — -zsh — 125×44 2025-04-03 17-14-19" style="zoom: 33%;" />



## Operación de 'pull' de la imagen de Docker Hub

Para bajar la imagen voy a utilizar Docker Desktop:

<img src="./Ejercicio%203.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-04-03%2017-18-30(1).png" alt="Monosnap Images - Docker Desktop 2025-04-03 17-18-30(1)" style="zoom:33%;" />

Y de esta forma vuelve a aparecer en mi Docker local:

<img src="./Ejercicio%203.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-04-03%2017-19-48(1).png" alt="Monosnap Images - Docker Desktop 2025-04-03 17-19-48(1)" style="zoom: 25%;" />



## Creación de un nuevo contenedor con la imagen, cambiando el puerto del contenedor.

Para crear un nuevo contenedor con la imagen que he bajado, le indico:

```bash
docker run -d -p 1234:80 pfany/new-image
```

pero desde la interfaz de Docker Desktop:

<img src="./Ejercicio%203.assets/new-image%20-%20Image%20-%20Docker%20Desktop%202025-04-03%2017-24-00.png" alt="new-image - Image - Docker Desktop 2025-04-03 17-24-00" style="zoom:25%;" />

Estos son los contenedores:

<img src="./Ejercicio%203.assets/Monosnap%20Containers%20-%20Docker%20Desktop%202025-04-03%2017-26-51(1).png" alt="Monosnap Containers - Docker Desktop 2025-04-03 17-26-51(1)" style="zoom:25%;" />

Si en esa línea hago click en los 3 puntitos, se abre en el browser:

<img src="./Ejercicio%203.assets/Monosnap%20The%20funniest%20web%202025-04-03%2017-28-10.png" alt="Monosnap The funniest web 2025-04-03 17-28-10" style="zoom:33%;" />

Y al hacer click en el `Script de fecha y hora´:

<img src="./Ejercicio%203.assets/fecha.php%202025-04-03%2017-29-26.png" alt="fecha.php 2025-04-03 17-29-26" style="zoom:33%;" />

Vemos la salida del script `fecha.php` y de la página `index.html` en el navegador.

