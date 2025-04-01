# Proyecto WordPress con Docker

Este proyecto utiliza Docker y Docker Compose para levantar una instancia de WordPress con una base de datos MariaDB.

## Requisitos

*   **Docker:** Asegúrate de tener Docker instalado en tu sistema. Puedes descargarlo desde [https://www.docker.com/get-started](https://www.docker.com/get-started).
*   **Docker Compose:** Docker Compose suele venir incluido con la instalación de Docker Desktop. Si no lo tienes, puedes instalarlo por separado.
* **Terminal:** Necesitarás una terminal para ejecutar los comandos.

## Pasos para levantar el proyecto

1.  **Clonar el repositorio (si aplica):** Si este proyecto está en un repositorio Git, clónalo en tu máquina local. Si ya tienes los archivos en tu máquina, omite este paso.
    ```bash
    git clone https://github.com/florindofp/2-SMR-AW-WORDPRESS.git
    cd 2-SMR-AW-WORDPRESS
    ```
    Si no tienes el repositorio, simplemente navega a la carpeta donde tienes el archivo `docker-compose.yml`

2.  **Navegar al directorio del proyecto:** Asegúrate de estar en el directorio donde se encuentra el archivo `docker-compose.yml`. Por ejemplo, si el repositorio está en `/var/www/2smr-aw-wordpress/`.
    ```bash
    cd /var/www/2smr-aw-wordpress/
    ```

3.  **Levantar los contenedores:** Ejecuta el siguiente comando para construir y levantar los contenedores definidos en `docker-compose.yml`. El parámetro `-d` indica que los contenedores se ejecutarán en segundo plano (detached mode).
    ```bash
    docker-compose up -d
    ```
    Este comando hará lo siguiente:
    *   Descargará las imágenes de `mariadb:latest` y `wordpress:latest` si no están presentes en tu máquina.
    *   Creará una red llamada `wp_network`.
    *   Creará un volumen llamado `.db_data` para la base de datos y `wp_data` para los archivos de wordpress.
    *   Creará y ejecutará dos contenedores: `wordpress_db` (MariaDB) y `wordpress_app` (WordPress).
    *   Configurará la base de datos con las credenciales especificadas en el archivo `docker-compose.yml`.
    *   Configurará WordPress para conectarse a la base de datos.
    *   Expondrá el puerto 80 del contenedor `wordpress_app` al puerto 80 de tu máquina local.

4.  **Acceder a WordPress:** Una vez que los contenedores estén en funcionamiento, puedes acceder a tu instalación de WordPress abriendo tu navegador web y visitando `http://localhost`.

5. **Configurar WordPress:** La primera vez que accedas a `http://localhost`, serás redirigido a la página de configuración de WordPress. Sigue las instrucciones para completar la instalación.

## Comandos útiles

*   **Detener los contenedores:**
    ```bash
    docker-compose down
    ```
    Esto detendrá y eliminará los contenedores, pero conservará los volúmenes de datos.

*   **Detener y eliminar los contenedores y volúmenes:**
    ```bash
    docker-compose down -v
    ```
    Esto detendrá y eliminará los contenedores y también los volúmenes de datos. **Ten cuidado con este comando, ya que perderás todos los datos de la base de datos y de WordPress.**

*   **Ver el estado de los contenedores:**
    ```bash
    docker-compose ps
    ```

*   **Ver los logs de un contenedor:**
    ```bash
    docker-compose logs -f wordpress
    ```
    Reemplaza `wordpress` con el nombre del servicio del que quieres ver los logs (por ejemplo, `db`).

*   **Reiniciar los contenedores:**
    ```bash
    docker-compose restart
    ```

* **Reconstruir las imagenes:**
    ```bash
    docker-compose up -d --build
    ```
    Esto es util si se han modificado los archivos de las imagenes.

## Notas

*   **Credenciales de la base de datos:**
    *   **Usuario:** `wpuser`
    *   **Contraseña:** `wppassword`
    *   **Base de datos:** `wordpress`
    *   **Root Password:** `rootpassword`
*   Los datos de la base de datos se almacenan en el volumen `./.db_data` en tu máquina local.
*   Los archivos de WordPress se almacenan en el volumen `./wp_data` en tu máquina local.
*   Si necesitas cambiar las credenciales de la base de datos, modifica el archivo `docker-compose.yml` y luego ejecuta `docker-compose down` y `docker-compose up -d` para aplicar los cambios.
* Si se quiere cambiar el puerto de acceso, se debe modificar la linea `ports` del servicio `wordpress` en el archivo `docker-compose.yml`. Por ejemplo, si se quiere acceder por el puerto 8080, se debe modificar a `8080:80`.
