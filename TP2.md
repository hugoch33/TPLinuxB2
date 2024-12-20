# TP2 : Utilisation courante de Docker

## TP2 Commun : Stack Web

🌞 docker-compose.yml

```
PS C:\Users\chama\TP2_Linux> cat .\docker-compose.yml
services:
  web:
    image: python
    environment:
      PORT: 80
      DB_HOST: "mysql"
    ports:
      - "8080:80"

  mysql:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - ./seed.sql:/docker-entrypoint-initdb.d/seed.sql

  phpmyadmin:
    image: phpmyadmin
    environment:
      PMA_HOST: db
    ports:
     - "8081:80"
```

# TP2 admins : Web stack

🌞 Limiter l'accès aux ressources

```
PS C:\Users\chama\TP2_Linux> cat .\docker-compose.yml
services:
  web:
    image: python
    deploy:
      resources:
        limits:
          cpus: '1'
          memory: 1G
    environment:
      PORT: 80
      DB_HOST: "mysql"
    ports:
      - "8080:80"

  mysql:
    image: mysql:latest
    deploy:
      resources:
        limits:
          cpus: '1'
          memory: 1G
    environment:
      MYSQL_ROOT_PASSWORD: root
    volumes:
      - ./seed.sql:/docker-entrypoint-initdb.d/seed.sql

  phpmyadmin:
    image: phpmyadmin
    deploy:
      resources:
        limits:
          cpus: '1'
          memory: 1G
    environment:
      PMA_HOST: mysql
    ports:
     - "8081:80"
```

🌞 No root

🌞 Gestion des droits du volume qui contient le code

```
PS C:\Users\chama\TP2_Linux> cat .\Dockerfile
FROM nginx

RUN apt update -y

RUN chown -R nginx:nginx /etc/nginx

RUN chown -R nginx:nginx /var/cache/nginx

RUN touch /var/run/nginx.pid

RUN chown nginx:nginx /var/run/nginx.pid

USER nginx
```

# II. Reverse proxy buddy

🌞 Adaptez le docker-compose.yml de la partie précédente

