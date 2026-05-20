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
