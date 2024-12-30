# Local Metabase Setup

This repository contains the configuration files needed to run Metabase locally using Docker.

## Prerequisites

- Docker
- Docker Compose
- At least 2GB of free RAM
- At least 2GB of free disk space

## Quick Start

1. Clone this repository:
```bash
git clone <repository-url>
cd <repository-name>
```

2. Create necessary directories:
```bash
mkdir -p metabase-data
```

3. Start the services:
```bash
docker-compose up -d
```

4. Access Metabase:
- Open your browser and navigate to http://localhost:3000
- Follow the initial setup wizard to configure your admin account

## Monitoring

View logs:
```bash
# All services
docker-compose logs -f

# Just Metabase
docker-compose logs -f metabase

# Just PostgreSQL
docker-compose logs -f postgres
```

## Stopping the Services

```bash
docker-compose down
```

To remove all data and start fresh:
```bash
docker-compose down -v
```

## Configuration

The default configuration is stored in the `.env` file. You can modify these values before starting the services:

- `POSTGRES_USER`: Database user (default: metabase)
- `POSTGRES_PASSWORD`: Database password (default: metabase123)
- `POSTGRES_DB`: Database name (default: metabase)

## Data Persistence

Data is persisted in the following locations:
- PostgreSQL data: Docker volume `postgres-data`
- Metabase data: `./metabase-data` directory

## Security Note

The default credentials in the `.env` file are for local development only. 
If you plan to use this setup in a production environment, make sure to:
1. Change all passwords
2. Use proper SSL/TLS configuration
3. Follow security best practices

For more detailed instructions, refer to `docs/instructions.md`.