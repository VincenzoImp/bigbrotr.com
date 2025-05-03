---
title: Getting Started
description: A step-by-step guide to set up your Bigbrotr instance.
---

This guide helps you quickly set up the **BigBrotr** environment using Docker Compose, including PostgreSQL and pgAdmin.

## 1. 🔁 Clone the Repository

Download the BigBrotr project to your local machine:

```bash
git clone https://github.com/VincenzoImp/bigbrotr.git
cd bigbrotr
```

---

## 2. ⚙️ Create the `.env` File

Copy the example environment file:

```bash
cp .env.example .env
```

🔧 *You can edit `.env` to customize credentials, ports, or file paths as needed.*

---

## 3. 🗂️ Create the Data Directory

Make sure the data directory exists, where PostgreSQL will store persistent data:

```bash
mkdir -p data
```

💡 *Optionally, you can add SQL commands to `init.sql` to initialize the database.*

---

## 4. 🐳 Start the Containers

Run Docker Compose using the `.env` file:

```bash
docker compose --env-file .env up -d
```

✔️ This will launch:

- **PostgreSQL** (`bigbrotr_db`) on the port defined in `.env` (default: `5432`)
- **pgAdmin** (`bigbrotr_pgadmin`) accessible at [http://localhost:8080](http://localhost:8080)

👤 *Log in to pgAdmin using the email and password from your `.env` file.*

---

## 5. 🛑 Stop the Containers

To shut everything down:

```bash
docker compose down
```

❗ To remove containers **and** volumes (⚠️ this will delete database data):

```bash
docker compose down -v
```

---