# TAS6 - Wordpress con docker compose YML
## 1. Título
**Implementación de un entorno WordPress usando Docker Compose: WordPress + MySQL + phpMyAdmin**

## 2. Tiempo de duración
20 minutos

## 3. Fundamentos
se utiliza Docker Compose, una herramienta que permite definir múltiples servicios en un solo archivo YAML. En esta práctica se implementa un stack compuesto por tres servicios:

- WordPress: Un CMS (Sistema de Gestión de Contenidos) que funciona sobre PHP.
- MySQL: Motor de base de datos que WordPress usa para almacenar usuarios, posts y configuraciones.
- phpMyAdmin: Interfaz web para administrar la base de datos MySQL.

Docker Compose permite establecer relaciones entre servicios mediante una red interna, lo cual garantiza la comunicación entre ellos. Además, se utilizan volúmenes, que son unidades de almacenamiento persistente que permiten mantener la información aunque los contenedores se eliminen o reinicien.

## 4. Conocimientos previos
- Uso básico de comandos Linux: cd, ls, mkdir, docker ps, docker compose up -d
- Manejo de navegador web
- Conocimiento básico de bases de datos
- Saber abrir terminal PowerShell / CMD
- Lectura básica de archivos YAML

## 5. Objetivos a alcanzar
- Implementar contenedores conectados entre sí mediante Docker Compose.
- Desplegar WordPress junto con MySQL y phpMyAdmin.
- Manipular el archivo de configuración docker-compose.yml.
- Verificar interacción entre los servicios a través de una red interna.
- Comprobar persistencia con volúmenes.

## 6. Equipo necesario
- Docker Desktop 
- Editor de texto 
- Navegador 

## 7. Material de apoyo
- Documentación oficial de Docker: https://docs.docker.com
- Documentación de WordPress Docker: https://hub.docker.com/_/wordpress
- Documentación de MySQL Docker: https://hub.docker.com/_/mysql
- Documentación de phpMyAdmin Docker: https://hub.docker.com/_/phpmyadmin
- Guía de la asignatura

## 8. Procedimiento

#### Paso 1: Crear el archivo docker-compose.yml
Crear un archivo llamado docker-compose.yml con el siguiente contenido:

![Figura 8-1.](img/Screenshot%202025-11-15%20232937.png)

```powershell
version: "3.9"

services:
  wordpress:
    image: wordpress:latest
    container_name: wordpress_app
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: mysql:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppassword
      WORDPRESS_DB_NAME: wpdb
    depends_on:
      - mysql
    networks:
      - wpnet
    volumes:
      - wp_data:/var/www/html

  mysql:
    image: mysql:5.7
    container_name: mysql_db
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wpdb
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppassword
    networks:
      - wpnet
    volumes:
      - mysql_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin_service
    ports:
      - "9090:80"
    environment:
      PMA_HOST: mysql
      PMA_USER: wpuser
      PMA_PASSWORD: wppassword
    depends_on:
      - mysql
    networks:
      - wpnet

networks:
  wpnet:

volumes:
  wp_data:
  mysql_data:

```

#### Paso 2: Ejecutar Docker Compose desde la terminal
Abrir la terminal dentro de la carpeta y ejecutar:

```powershell
docker compose up -d
```

![Figura 8-3.](img/Screenshot%202025-11-15%20230922.png)

#### Paso 3: Abrir WordPress y phpMyAdmin
En el navegador escribir:
```powershell
http://localhost:8080
```

![Figura 8-1.](img/Screenshot%202025-11-15%20231026.png)

```powershell
http://localhost:9090
```

![Figura 8-1.](img/Screenshot%202025-11-15%20231042.png)

## 9. Resultados esperados
La práctica ha demostrado satisfactoriamente el mecanismo de persistencia de datos en Docker.

- Contenedores funcionando correctamente:
    - wordpress_app
    - mysql_db
    - phpmyadmin_service
- Acceso completo a WordPress desde localhost:8080
- Acceso al administrador de base de datos desde localhost:9090
- Persistencia de datos verificada gracias a los volúmenes.
- Comunicación correcta entre WordPress ↔ MySQL ↔ phpMyAdmin.

## 10. Bibliografía
- Docker, Inc. (2024). Docker documentation. https://docs.docker.com
- Docker, Inc. (2024). WordPress – Official Docker image. https://hub.docker.com/_/wordpress
- Docker, Inc. (2024). MySQL – Official Docker image. https://hub.docker.com/_/mysql
- Docker, Inc. (2024). phpMyAdmin – Official Docker image. https://hub.docker.com/_/phpmyadmin
- MySQL. (2024). MySQL documentation. https://dev.mysql.com/doc
- WordPress Foundation. (2024). WordPress documentation. https://wordpress.org/support
- phpMyAdmin Team. (2024). phpMyAdmin documentation. https://docs.phpmyadmin.net