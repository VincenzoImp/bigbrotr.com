---
title: Getting Started
description: A step-by-step guide to set up your Bigbrotr instance.
---

This guide helps you quickly set up the **BigBrotr** instance using Docker Compose, including PostgreSQL and pgAdmin.

## 1. Clone the Repository

Download the BigBrotr repository to your local machine:

```bash
git clone https://github.com/VincenzoImp/bigbrotr.git
cd bigbrotr
```

---

## 2. Create the `.env` File

Copy the example environment file:

```bash
cp .env.example .env
```

🔧 *You can edit `.env` to customize credentials, ports, or file paths as needed.*

#### Explanation of `.env` File

The `.env` file contains environment variables that will configure your Docker containers for PostgreSQL and pgAdmin. Here’s a breakdown of the variables in `.env`:

```env
# Database Host
DB_HOST=localhost

# Database Configuration
POSTGRES_USER=admin
POSTGRES_PASSWORD=admin
POSTGRES_DB=bigbrotr
POSTGRES_DB_DATA_PATH=./data
POSTGRES_DB_INIT_PATH=./init.sql

# Ports
DB_PORT=5432
PGADMIN_PORT=8080

# pgAdmin Configuration
PGADMIN_DEFAULT_EMAIL=admin@admin.com
PGADMIN_DEFAULT_PASSWORD=admin
```

- **DB_HOST**: This is the hostname for your database. In most setups, this would be `localhost` unless you're setting up a multi-host environment.
- **POSTGRES_USER**: The username for the PostgreSQL instance.
- **POSTGRES_PASSWORD**: The password for the PostgreSQL user.
- **POSTGRES_DB**: The name of the default database to create.
- **POSTGRES_DB_DATA_PATH**: Path to the local directory where PostgreSQL will store its data files. This is set to `./data` by default.
- **POSTGRES_DB_INIT_PATH**: Path to an optional SQL file that can be used to initialize the database. The default path is `./init.sql`.

🔧 *You can customize the values of these variables to fit your needs. For instance, change the `POSTGRES_USER` and `POSTGRES_PASSWORD` to more secure values, and if you're running PostgreSQL on a different host or port, update `DB_HOST` and `DB_PORT` accordingly.*

---

## 3. Create the Data Directory

Make sure the data directory exists, where PostgreSQL will store persistent data:

```bash
mkdir -p data
```

💡 *Optionally, you can add SQL commands to `init.sql` to initialize the database.*

---

## 4. Start the Containers

Run Docker Compose using the `.env` file:

```bash
docker compose --env-file .env up -d
```

✔️ This will launch:

- **PostgreSQL** (`bigbrotr_db`) on the port defined in `.env` (default: `5432`)
- **pgAdmin** (`bigbrotr_pgadmin`) accessible at [http://localhost:8080](http://localhost:8080)

👤 *Log in to pgAdmin using the email and password from your `.env` file.*

#### Docker Compose Breakdown

Here’s how the Docker Compose file works in conjunction with the `.env` file:

```yaml
services:

  db:
    image: postgres:latest
    container_name: bigbrotr_db
    environment:
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
    ports:
      - "${DB_PORT}:${DB_PORT}"
    volumes:
      - ${POSTGRES_DB_DATA_PATH}:/var/lib/postgresql/data
      - ${POSTGRES_DB_INIT_PATH}:/docker-entrypoint-initdb.d/init.sql
    networks:
      - bigbrotr_network
    restart: unless-stopped

  pgadmin:
    image: dpage/pgadmin4
    container_name: bigbrotr_pgadmin
    environment:
      - PGADMIN_DEFAULT_EMAIL=${PGADMIN_DEFAULT_EMAIL}
      - PGADMIN_DEFAULT_PASSWORD=${PGADMIN_DEFAULT_PASSWORD}
    ports:
      - "${PGADMIN_PORT}:80"
    networks:
      - bigbrotr_network
    depends_on:
      - db
    restart: unless-stopped

networks:
  bigbrotr_network:
    driver: bridge

volumes:
  bigbrotr_data:
    driver: local
```

- The `db` service uses the official PostgreSQL image.
- It pulls the environment variables directly from your `.env` file for configuration (`POSTGRES_USER`, `POSTGRES_PASSWORD`, etc.).
- It maps the port `DB_PORT` and mounts two volumes:
    - `${POSTGRES_DB_DATA_PATH}` maps to PostgreSQL's data storage directory.
    - `${POSTGRES_DB_INIT_PATH}` maps to an initialization SQL script.
- The `pgadmin` service uses the `dpage/pgadmin4` image.
- It reads the environment variables from the `.env` file (`PGADMIN_DEFAULT_EMAIL`, `PGADMIN_DEFAULT_PASSWORD`).
- It maps the `PGADMIN_PORT` to port `80` inside the container, allowing you to access pgAdmin through `http://localhost:8080`.

---

## 5. Stop the Containers

To shut everything down:

```bash
docker compose down
```

❗ To remove containers **and** volumes (⚠️ this will delete database data):

```bash
docker compose down -v
```

---