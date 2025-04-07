---
Author: Epifanía Peralta López
Title: Ejercicio 1
typora-copy-images-to: ./Ejercicio1.assets
---

# Ejercicio 1 - Contenedores en red y Docker Desktop

> Documento que resuelve el ejercicio 1 de la Tarea 3- Despliegue de Aplicaciones Web, realizado por Epifanía Peralta López.

[TOC]

## Notas

Este ejercicio se resuelve con Docker Desktop.

Si se necesitara utilizar comandos, se usa la Terminal integrada en Desktop.

Se sugiere utilizar la extensión **PortNavigator** para gestionar las redes en Docker Desktop.



## Resolución del ejercicio.



1. **Enunciado:** Crea una red bridge **redej1**

Para crear la red bridge utilizo la extensión PortNavigator:

<img src="./Ejercicio1.assets/Monosnap%20extension%20-%20Docker%20Desktop%202025-03-29%2015-34-51(1).png" alt="Monosnap extension - Docker Desktop 2025-03-29 15-34-51(1)" style="zoom:25%;" />

Al hacer click en "Add Network" se crea la red que hemos llamado **redej1**

<img src="./Ejercicio1.assets/Monosnap%20extension%20-%20Docker%20Desktop%202025-03-29%2015-43-30.png" alt="Monosnap extension - Docker Desktop 2025-03-29 15-43-30" style="zoom:33%;" />



Las redes definidas por el usuario (por nosotros)  nos ofrece un servidor DNS que reconoce los nombres de los contenedores. Por éso, cuando conectemos los dos contenedores, se puede hacer por el nombre. Podrían conectarse a través de la IP, pero nadie garantizaría que si en un momento dado se borra el contenedor de mariaDB (porque se realice por ejemplo una actualización) y luego se vuelve a crear, que tome la misma dirección IP.



2. **Enunciado:** Crea un contenedor con una imagen de **mariadb** que estará en la red **redej1**. Este contenedor se ejecutará en segundo plano, y será accesible a través del puerto 3306.

* Definir una contraseña para el usuario **root**, y un **usuario con tu nombre de pila** y con contraseña. La BD por defecto será **DAW**.
* Genera un script SQL que cree una tabla **módulos** con algunos registros con los nombres de los módulos que estás estudiando.

En Docker Desktop se puede acceder a Docker Hub, de donde se obtiene la imagen oficial de mariaDB:

![Monosnap Docker Hub - Docker Desktop 2025-03-28 23-55-08](./Ejercicio1.assets/Monosnap%20Docker%20Hub%20-%20Docker%20Desktop%202025-03-28%2023-55-08.png)

Además, las variables de entorno que se necesitan son:

![Monosnap Image 2025-03-29 13-08-37(1)](./Ejercicio1.assets/Monosnap%20Image%202025-03-29%2013-08-37(1).png)

Así, cuando se cree el contenedor, al haber ya definido esas 4 variables de entorno, antes de ejecutar el servicio se configura y se crean el usuario, contraseña y base de datos.

Los volúmenes y los bind mounts se configuran en el momento de crear el contenedor, no después. Con Docker Desktop, antes de arrancar el contenedor.

> Estoy utilizando un macOS: voy a crear una carpeta en mi sistema donde se guardarán los datos de mariaDB.

<img src="./Ejercicio1.assets/Monosnap%20fanyperalta%20%E2%80%94%20-zsh%20%E2%80%94%2098%C3%9724%202025-03-29%2016-27-45(1).png" alt="Monosnap fanyperalta — -zsh — 98×24 2025-03-29 16-27-45(1)" style="zoom:33%;" />

Ahora los pasos son los siguientes:

* selecciono la imagen `mariadb`:

<img src="./Ejercicio1.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-03-29%2016-32-17(1).png" alt="Monosnap Images - Docker Desktop 2025-03-29 16-32-17(1)" style="zoom:33%;" />

* Una vez que tengo la imagen -> click en Run y despliego Opciones avanzadas:

<img src="./Ejercicio1.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-03-29%2016-42-44(1).png" alt="Monosnap Images - Docker Desktop 2025-03-29 16-42-44(1)" style="zoom:33%;" />

* Se abren las Optional settings, para poder nombrar al contenedor, indicar el puerto, el almacenamiento y las 4 variables de entorno:

<img src="./Ejercicio1.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-03-29%2019-50-31.png" alt="Monosnap Images - Docker Desktop 2025-03-29 19-50-31" style="zoom:33%;" />

<img src="./Ejercicio1.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-03-29%2019-51-35.png" alt="Monosnap Images - Docker Desktop 2025-03-29 19-51-35" style="zoom:33%;" />

Así, el contenedor ya está creado. Lo compruebo:

<img src="./Ejercicio1.assets/Monosnap%20Containers%20-%20Docker%20Desktop%202025-03-29%2019-58-50.png" alt="Monosnap Containers - Docker Desktop 2025-03-29 19-58-50" style="zoom:33%;" />

* En Docker Desktop no se puede elegir la red directamente al crear el contenedor desde la interfaz gráfica. 

Una vez creado, la solución es utilizar la terminal incluída en Docker Desktop y escribir:

```bash
docker network connect redej1 mariadb_tarea3
```

Sería así:

<img src="./Ejercicio1.assets/Monosnap%20Containers%20-%20Docker%20Desktop%202025-03-29%2020-10-23.png" alt="Monosnap Containers - Docker Desktop 2025-03-29 20-10-23" style="zoom:33%;" />

> Corrección: se puede conectar la red **redej1** al contenedor **mariadb_tarea3** a través de la extensión instalada de **Portnavigator**.

<img src="./Ejercicio1.assets/Monosnap%20extension%20-%20Docker%20Desktop%202025-03-29%2020-36-50.png" alt="Monosnap extension - Docker Desktop 2025-03-29 20-36-50" style="zoom:33%;" />



* Por último, en Action doy click en el triangulito para arrancar el contenedor.  En Stats, compruebo que todo funciona:

<img src="./Ejercicio1.assets/Monosnap%20mariadb_tarea3%20-%20Container%20-%20Docker%20Desktop%202025-03-29%2020-13-53.png" alt="Monosnap mariadb_tarea3 - Container - Docker Desktop 2025-03-29 20-13-53" style="zoom:33%;" />



* Dentro de la carpeta Ejercicio1 he guardado el archivo `init.sql`  que he creado en SQL, con la tabla `módulos` que contiene varios registros con los módulos que estoy estudiando.

<img src="./Ejercicio1.assets/Monosnap%20init.sql%20%E2%80%94%20DespliegueTarea3%202025-03-29%2020-18-36.png" alt="Monosnap init.sql — DespliegueTarea3 2025-03-29 20-18-36" style="zoom:33%;" />



3. **Enunciado:** Crea un contenedor con **Adminer** o con **phpMyAdmin** que se pueda conectar al contenedor de la BD.

Compruebo la info de la imagen de **phpMyAdmin** desde Docker Desktop:

<img src="./Ejercicio1.assets/Monosnap%20Global%20Search%20-%20Docker%20Desktop%202025-03-29%2021-29-24.png" alt="Monosnap Global Search - Docker Desktop 2025-03-29 21-29-24" style="zoom:33%;" />

Los pasos a seguir para crear el contenedor **phpmyadmin_tarea3** son:

* Descargo la imagen anterior y accedo a las 

<img src="./Ejercicio1.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-03-29%2021-39-07(1).png" alt="Monosnap Images - Docker Desktop 2025-03-29 21-39-07(1)" style="zoom:33%;" />

* Incorporo los datos:
  * Puerto: 8080:80, para acceder desde el navegador en `http://localhost:8080`
  * Variables de entorno:
    * PMA_HOST=mariadb_tarea3, que es el nombre que le dí al contenedor de mariaDB como servidor de base de datos.
    * MYSQL_ROOT_PASSWORD=admin, que es la contraseña que puse para **root** en mariadb_tarea3.

<img src="./Ejercicio1.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-03-29%2021-44-53.png" alt="Monosnap Images - Docker Desktop 2025-03-29 21-44-53" style="zoom:33%;" />

<img src="./Ejercicio1.assets/Monosnap%20Images%20-%20Docker%20Desktop%202025-03-29%2021-45-30.png" alt="Monosnap Images - Docker Desktop 2025-03-29 21-45-30" style="zoom:33%;" />

* Conecto phpmyadmin_tarea3 a la red redej1, utilizando el PortNavigator:
  * Primero tuve que cerrar Docker Desktop, como para que actualizara, porque no me salía el nuevo contenedor en ninguna red.
  * Al volver a abrirlo, ya me sale que está en la red `bridge`, por lo que lo desconecto de ahí.

<img src="./Ejercicio1.assets/Monosnap%20extension%20-%20Docker%20Desktop%202025-03-29%2022-11-21(1).png" alt="Monosnap extension - Docker Desktop 2025-03-29 22-11-21(1)" style="zoom:33%;" />

​			* Y ahora ya puedo conectarlo a la red `redej1`:

<img src="./Ejercicio1.assets/Monosnap%20extension%20-%20Docker%20Desktop%202025-03-29%2021-51-36.png" alt="Monosnap extension - Docker Desktop 2025-03-29 21-51-36" style="zoom:33%;" />

> Directamente no deja `Add Container` a la `redej1`. Me dí cuenta que en Inspect del `phpmyadmin_tare3` en `Network` figuraba **none**, por lo que en el PortNavigator me fuí a esa red y ahí estaba. Desde ahí es donde me permitió conectarlo a la `redej1`.



Los contenedores creados y en ejecución son:

<img src="./Ejercicio1.assets/Monosnap%20Containers%20-%20Docker%20Desktop%202025-03-29%2022-26-59.png" alt="Monosnap Containers - Docker Desktop 2025-03-29 22-26-59" style="zoom:33%;" />

Los parámetros de la red bridge utilizada: IPs

<img src="./Ejercicio1.assets/Monosnap%20extension%20-%20Docker%20Desktop%202025-03-29%2022-29-35.png" alt="Monosnap extension - Docker Desktop 2025-03-29 22-29-35" style="zoom:33%;" />

<img src="./Ejercicio1.assets/Monosnap%20extension%20-%20Docker%20Desktop%202025-03-29%2022-30-35.png" alt="Monosnap extension - Docker Desktop 2025-03-29 22-30-35" style="zoom:33%;" />



3. **Enunciado:** Desde la interfaz gráfica de `phpMyAdmin`, conectarse a la BD con el usuario personal, ejecuta el script con los datos de los módulos y muestra la BD y la tabla creados.

Desde Containers puedo acceder al navegador, haciendo click en los 3 puntitos según indico:

<img src="./Ejercicio1.assets/Monosnap%20Containers%20-%20Docker%20Desktop%202025-03-29%2022-35-15(1).png" alt="Monosnap Containers - Docker Desktop 2025-03-29 22-35-15(1)" style="zoom:33%;" />

En el navegador se abre phpMyAdmin:

<img src="./Ejercicio1.assets/Monosnap%20phpMyAdmin%202025-03-29%2022-37-53.png" alt="Monosnap phpMyAdmin 2025-03-29 22-37-53" style="zoom:33%;" />

Al introducir los datos de usuario `fany` y la contraseña indicada al crear el contenedor de `mariadb_tarea3` :

<img src="./Ejercicio1.assets/Monosnap%20localhost8080%20%20mariadb_tarea3%20%20DAW%20%20phpMyAdmin%205.2.2%202025-03-29%2022-39-40(1).png" alt="Monosnap localhost8080  mariadb_tarea3  DAW  phpMyAdmin 5.2.2 2025-03-29 22-39-40(1)" style="zoom:33%;" />

Para ejecutar el script del archivo `init.sql` primeramente he de importarlo a la base de datos:

<img src="./Ejercicio1.assets/Monosnap%20localhost8080%20%20mariadb_tarea3%20%20DAW%20%20phpMyAdmin%205.2.2%202025-03-29%2022-47-20(1).png" alt="Monosnap localhost8080  mariadb_tarea3  DAW  phpMyAdmin 5.2.2 2025-03-29 22-47-20(1)" style="zoom:33%;" />

Y finaliza la importación:

<img src="./Ejercicio1.assets/%20DAW%20%7C%20phpMyAdmin%205.2.2%202025-03-29%2022-49-17.png" alt=" DAW | phpMyAdmin 5.2.2 2025-03-29 22-49-17" style="zoom:33%;" />

Donde se puede ver la tabla **módulos**:

<img src="./Ejercicio1.assets/Monosnap%20localhost8080%20%20mariadb_tarea3%20%20DAW%20%20phpMyAdmin%205.2.2%202025-03-29%2022-55-33(1).png" alt="Monosnap localhost8080  mariadb_tarea3  DAW  phpMyAdmin 5.2.2 2025-03-29 22-55-33(1)" style="zoom:33%;" />

Y al hacer click en **módulos**:

<img src="./Ejercicio1.assets/%20modulos%20%7C%20phpMyAdmin%205.2.2%202025-03-29%2022-57-25.png" alt=" modulos | phpMyAdmin 5.2.2 2025-03-29 22-57-25" style="zoom:33%;" />



5. **Enunciado:** Elige entre estas dos opciones:
   1. Instala la extensión `Disk Usage`, muestra el espacio ocupado, borra algo...
   2. Instala la extensión `Resource usage` y muestra su salida cuando estén activos los contenedores.

Muestro el contenido de `Resource usage` instalado con los 2 contenedores activos:

<img src="./Ejercicio1.assets/Monosnap%20extension%20-%20Docker%20Desktop%202025-03-29%2023-02-10.png" alt="Monosnap extension - Docker Desktop 2025-03-29 23-02-10" style="zoom:33%;" />



* Borra los contenedores, la red y los volúmenes utilizados.

Para borrar los contenedores, lo primero es detenerlos, desde la pestaña de Containers.

<img src="./Ejercicio1.assets/Monosnap%20Containers%20-%20Docker%20Desktop%202025-04-07%2017-31-02(1).png" alt="Monosnap Containers - Docker Desktop 2025-04-07 17-31-02(1)" style="zoom:33%;" />

Y luego, click en el icono de la papelera para borrar el contenedor:

<img src="./Ejercicio1.assets/Monosnap%20Containers%20-%20Docker%20Desktop%202025-04-07%2017-32-43.png" alt="Monosnap Containers - Docker Desktop 2025-04-07 17-32-43" style="zoom:25%;" />

Realizo la misma operación con el contenedor de mariadb_tarea3:

<img src="./Ejercicio1.assets/Monosnap%20Containers%20-%20Docker%20Desktop%202025-04-07%2017-33-53.png" alt="Monosnap Containers - Docker Desktop 2025-04-07 17-33-53" style="zoom:25%;" />

Y una vez que la red `redej1` no tiene contenedores conectados, puedo borrarla,  cerrando la ventana en el `x` de la esquina superior derecha:

<img src="./Ejercicio1.assets/Monosnap%20extension%20-%20Docker%20Desktop%202025-04-07%2017-35-47.png" alt="Monosnap extension - Docker Desktop 2025-04-07 17-35-47" style="zoom:25%;" />

En cuanto a los volúmenes, en mi caso usé un Bind Mount, así que no aparece como volumen en Docker Desktop:

<img src="./Ejercicio1.assets/Monosnap%20Volumes%20-%20Docker%20Desktop%202025-04-07%2017-38-01.png" alt="Monosnap Volumes - Docker Desktop 2025-04-07 17-38-01" style="zoom:25%;" />

El directorio que usé  para el Bind Mount figura en mi sistema de archivos (fuera de Docker): `users/fanyperalta/docker/mariadb_data` y en la carpeta DAW figura la tabla `módulos` creada.

<img src="./Ejercicio1.assets/Monosnap%20DAW%202025-04-07%2017-26-49.png" alt="Monosnap DAW 2025-04-07 17-26-49" style="zoom:25%;" />

En este caso, para borrar la carpeta `DAW` tengo que hacerlo manualmente desde la terminal o el Finder.

> Ejercicio1 de la Tarea3 de Despliegue.
