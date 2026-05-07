# 🚀 WordPress en Docker

Despliegue completo de **WordPress + MySQL 8** utilizando Docker Compose.  
Ideal para entornos de desarrollo, laboratorios, homelab o despliegues rápidos en servidores Linux.

# 📦 Características

- ✅ WordPress actualizado automáticamente (`latest`)
- ✅ Base de datos MySQL 8
- ✅ Persistencia de datos con volúmenes Docker
- ✅ Reinicio automático de contenedores
- ✅ Configuración sencilla y portable
- ✅ Acceso vía navegador web
- ✅ Fácil backup y migración

# 🧱 Arquitectura

```text
┌───────────────────────┐
│      WordPress        │
│    Puerto: 9300       │
└──────────┬────────────┘
           │
           ▼
┌───────────────────────┐
│       MySQL 8         │
│   Base de datos WP    │
└───────────────────────┘
```
# 📂 docker-compose.yml

```yaml
services:
  db:
    image: mysql:8
    container_name: wordpress_db
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: Passw0rd
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wp_user
      MYSQL_PASSWORD: Passw0rd

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    container_name: wordpress_app
    volumes:
      - wordpress_data:/var/www/html
    ports:
      - '9300:80'
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: Passw0rd
      WORDPRESS_DB_NAME: wordpress

volumes:
  db_data: {}
  wordpress_data: {}
```

# ⚙️ Requisitos

Antes de comenzar asegúrate de tener instalado:

- Docker
- Docker Compose

# 🚀 Despliegue

## 1️⃣ Crear carpeta del proyecto

```bash
mkdir wordpress-docker
cd wordpress-docker
```

## 2️⃣ Crear el archivo compose

```bash
nano docker-compose.yml
```

Pega el contenido mostrado anteriormente.

## 3️⃣ Iniciar los contenedores

```bash
docker compose up -d
```

## 4️⃣ Verificar el estado

```bash
docker ps
```

Deberías ver:

- `wordpress_app`
- `wordpress_db`

# 🌐 Acceso

Abre el navegador y accede a:

```text
http://IP_DEL_SERVIDOR:9300
```

Ejemplo:

```text
http://192.168.1.100:9300
```

# 🔐 Credenciales de base de datos

| Parámetro | Valor |
|---|---|
| Base de datos | wordpress |
| Usuario | wp_user |
| Contraseña | Passw0rd |

# 💾 Persistencia de datos

Los datos se almacenan en los volúmenes:

- `db_data`
- `wordpress_data`

Esto permite mantener:

- Base de datos
- Plugins
- Temas
- Subidas multimedia
- Configuración WordPress

Aunque elimines los contenedores.

# 🛠️ Comandos útiles

## Ver logs

```bash
docker logs -f wordpress_app
```

## Reiniciar servicios

```bash
docker compose restart
```

## Detener contenedores

```bash
docker compose down
```

## Eliminar completamente incluyendo volúmenes

```bash
docker compose down -v
```
# 🔒 Recomendaciones de seguridad

⚠️ Este despliegue es válido para laboratorio o entorno interno.

Para producción se recomienda:

- Cambiar contraseñas por valores seguros
- Usar HTTPS con Nginx Proxy Manager o Traefik
- Limitar acceso al puerto 9300
- Realizar backups periódicos
- Añadir fail2ban o firewall

# 📦 Actualización de imágenes

Actualizar contenedores:

```bash
docker compose pull
docker compose up -d
```

# 🧹 Backup rápido

## Backup base de datos

```bash
docker exec wordpress_db mysqldump -u root -p wordpress > backup.sql
```

## Backup archivos WordPress

```bash
docker run --rm -v wordpress_data:/data -v $(pwd):/backup alpine tar czf /backup/wordpress_data.tar.gz /data
```




