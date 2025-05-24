---
title: Getting Started
description: A step-by-step guide to set up your Bigbrotr instance.
---

This guide helps you quickly set up the **Bigbrotr** instance using Docker Compose, including PostgreSQL, pgAdmin, and additional services.

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
cp env.example .env
```

🔧 *You can edit `.env` to customize credentials, ports, file paths, and service parameters as needed.*

#### Explanation of `.env` File

The `.env` file contains environment variables to configure your Docker containers and Bigbrotr services. Here’s a breakdown:

```env
# Ports
DB_PORT=5432
PGADMIN_PORT=8080

# Nostr keypair
SECRET_KEY=
PUBLIC_KEY=

# Database configuration
POSTGRES_USER=admin
POSTGRES_PASSWORD=admin
POSTGRES_DB=bigbrotr
POSTGRES_DB_DATA_PATH=./data
POSTGRES_DB_INIT_PATH=./init.sql

# pgAdmin configuration
PGADMIN_DEFAULT_EMAIL=admin@admin.com
PGADMIN_DEFAULT_PASSWORD=admin

# Initializer configuration
RELAYS_SEED_PATH=.relays_seed.txt

# Monitor configuration
MONITOR_FREQUENCY_HOUR=8
MONITOR_NUM_CORES=2
MONITOR_CHUNK_SIZE=100
MONITOR_REQUESTS_PER_CORE=25
MONITOR_REQUEST_TIMEOUT=20

# Syncronizer configuration
SYNCRONIZER_NUM_CORES=4
SYNCRONIZER_CHUNK_SIZE=200
SYNCRONIZER_REQUESTS_PER_CORE=25
SYNCRONIZER_REQUEST_TIMEOUT=20
```

- `DB_PORT`: Port on your host to expose PostgreSQL (maps to container port 5432).
- `PGADMIN_PORT`: Port on your host to access pgAdmin (maps to container port 80).
- `SECRET_KEY` / `PUBLIC_KEY`: Your Nostr keypair credentials.
- `POSTGRES_USER` / `POSTGRES_PASSWORD` / `POSTGRES_DB`: Credentials and database name for PostgreSQL.
- `POSTGRES_DB_DATA_PATH`: Local directory for persistent PostgreSQL data.
- `POSTGRES_DB_INIT_PATH`: Optional SQL file for initializing the database.
- `PGADMIN_DEFAULT_EMAIL` / `PGADMIN_DEFAULT_PASSWORD`: Credentials for logging into pgAdmin.
- `RELAYS_SEED_PATH`: Path to relays seed file used by the initializer service.
- **Monitor and Syncronizer variables**: Configure performance and behavior of Bigbrotr's monitor and synchronizer services.

---

## 3. Start the Containers

Launch the full stack using Docker Compose with your `.env` file:

```bash
docker compose --env-file .env up -d
```

✔️ This will start:

- **PostgreSQL** (`bigbrotr_database`) on `${DB_PORT}` (default: `5432`)
- **pgAdmin** (`bigbrotr_pgadmin`) accessible at [http://localhost:${PGADMIN_PORT}](http://localhost:8080)
- **Tor proxy** (`bigbrotr_torproxy`)
- **Initializer** (`bigbrotr_initializer`) - initializes database from seed
- **Monitor** (`bigbrotr_monitor`) - main monitoring service
- **Syncronizer** (`bigbrotr_syncronizer`) - synchronizes data with relays

👤 *Log into pgAdmin using the email and password from your `.env`.*

---

## 4. Stop the Containers

To stop all running containers:

```bash
docker compose down
```

---

## Docker Compose Overview

Here is the relevant `docker-compose.yml` snippet used:

```yaml
services:

  database:
    image: postgres:latest
    container_name: bigbrotr_database
    environment:
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
    ports:
      - ${DB_PORT}:5432
    volumes:
      - ${POSTGRES_DB_DATA_PATH}:/var/lib/postgresql/data
      - ${POSTGRES_DB_INIT_PATH}:/docker-entrypoint-initdb.d/init.sql
    networks:
      - network
    restart: unless-stopped

  pgadmin:
    image: dpage/pgadmin4
    container_name: bigbrotr_pgadmin
    environment:
      - PGADMIN_DEFAULT_EMAIL=${PGADMIN_DEFAULT_EMAIL}
      - PGADMIN_DEFAULT_PASSWORD=${PGADMIN_DEFAULT_PASSWORD}
    ports:
      - ${PGADMIN_PORT}:80
    networks:
      - network
    depends_on:
      - database
    restart: unless-stopped

  torproxy:
    image: dperson/torproxy
    container_name: bigbrotr_torproxy
    restart: unless-stopped
    networks:
      - network

  initializer:
    build:
      context: .
      dockerfile: dockerfiles/initializer
    container_name: bigbrotr_initializer
    environment:
      - POSTGRES_HOST=database
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
      - POSTGRES_PORT=5432
      - RELAYS_SEED_PATH=relays_seed.txt
    volumes:
      - ${RELAYS_SEED_PATH}:/app/relays_seed.txt
    depends_on:
      - database
    networks:
      - network
    restart: no

  monitor:
    build:
      context: .
      dockerfile: dockerfiles/monitor
    container_name: bigbrotr_monitor
    environment:
      - POSTGRES_HOST=database
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
      - POSTGRES_PORT=5432
      - TORPROXY_HOST=torproxy
      - TORPROXY_PORT=9050
      - MONITOR_FREQUENCY_HOUR=${MONITOR_FREQUENCY_HOUR}
      - MONITOR_NUM_CORES=${MONITOR_NUM_CORES}
      - MONITOR_CHUNK_SIZE=${MONITOR_CHUNK_SIZE}
      - MONITOR_REQUESTS_PER_CORE=${MONITOR_REQUESTS_PER_CORE}
      - MONITOR_REQUEST_TIMEOUT=${MONITOR_REQUEST_TIMEOUT}
      - SECRET_KEY=${SECRET_KEY}
      - PUBLIC_KEY=${PUBLIC_KEY}
    depends_on:
      - database
      - torproxy
    networks:
      - network
    restart: unless-stopped

  syncronizer:
    build:
      context: .
      dockerfile: dockerfiles/syncronizer
    container_name: bigbrotr_syncronizer
    environment:
      - POSTGRES_HOST=database
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
      - POSTGRES_DB=${POSTGRES_DB}
      - POSTGRES_PORT=5432
      - TORPROXY_HOST=torproxy
      - TORPROXY_PORT=9050
      - SYNCRONIZER_NUM_CORES=${SYNCRONIZER_NUM_CORES}
      - SYNCRONIZER_CHUNK_SIZE=${SYNCRONIZER_CHUNK_SIZE}
      - SYNCRONIZER_REQUESTS_PER_CORE=${SYNCRONIZER_REQUESTS_PER_CORE}
      - SYNCRONIZER_REQUEST_TIMEOUT=${SYNCRONIZER_REQUEST_TIMEOUT}
    depends_on:
      - database
      - torproxy
    networks:
      - network
    restart: unless-stopped

networks:
  network:
    driver: bridge
```