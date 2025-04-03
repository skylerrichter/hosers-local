# 🚀 hose.rs

This repository provides a Docker-based setup to run a project with the following services:

- **PostgreSQL** (`db`)
- **ZITADEL** (`zitadel`) – an identity and access management system
- **App Server** (`server`) – your custom backend service

## 📦 Services Overview

### 🗃️ `db` (PostgreSQL)
- **Image**: `postgres`
- **Ports**: `5432:5432`
- **Data Persistence**: Uses a Docker volume `dbdata`
- **Configuration**: Environment variables loaded from `.db.env`

### 🔐 `zitadel`
- **Image**: `ghcr.io/zitadel/zitadel:latest`
- **Ports**: `8080:8080`
- **Startup Command**:
  ```sh
  start-from-init --masterkey "MasterkeyNeedsToHave32Characters" --tlsMode disabled
  ```
- **Depends on**: `db` (waits until the database is healthy)
- **Configuration**: Environment variables loaded from `.zitadel.env`

### 🖥️ `server`
- **Build**: Local folder `./server`
- **Ports**: `8181:8181`
- **Depends on**: `db` (waits until the database is healthy)
- **Configuration**: Environment variables loaded from `.server.env`

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git git@github.com:skylerrichter/hosers-local.git
cd hosers-local
```

### 2. Create Environment Files

Create the following files in the root directory:

- `.db.env`
- `.zitadel.env`
- `.server.env`

Example `.db.env`:
```env
POSTGRES_USER=youruser
POSTGRES_PASSWORD=yourpassword
POSTGRES_DB=yourdatabase
```

Refer to the [ZITADEL documentation](https://zitadel.com/docs) and your custom server's config for respective environment variables.

### 3. Start the Services

```bash
docker-compose up --build
```

Services will be accessible at:

- **ZITADEL**: http://localhost:8080
- **Server**: http://localhost:8181

### 4. Stopping the Services

```bash
docker-compose down
```

Use `-v` to also remove the volume if needed:
```bash
docker-compose down -v
```

## 🗃️ Volume

- `dbdata`: Persists PostgreSQL data across container restarts.

## 📄 License
