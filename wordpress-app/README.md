services:
  db:
    image: mariadb:10.6.4-focal
    command: '--default-authentication-plugin=mysql_native_password'
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=somewordpress
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=wordpress
      - MYSQL_PASSWORD=wordpress
    expose:
      - 3306
      - 33060
    networks:
      - web
  wordpress:
    image: wordpress:latest
    volumes:
      - wp_data:/var/www/html
    ports:
      - 8080:80
    restart: always
    environment:
      - WORDPRESS_DB_HOST=db
      - WORDPRESS_DB_USER=wordpress
      - WORDPRESS_DB_PASSWORD=wordpress
      - WORDPRESS_DB_NAME=wordpress
    networks:
      - web
volumes:
  db_data:
  wp_data:
networks:
  web:


-- 
Sao chép mã nguồn từ container ra máy host:
docker cp CONTAINER_ID:/var/www/html/ /path/to/local/destination/

ex: docker cp 3465d37c2a79:/var/www/html C:\Users\HUONG\Desktop\Docker\demo-docker-compose\app-example2