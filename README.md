# PostgreSQL with PgAdmin using Docker Compose

This repository provides a simple way to spin up a PostgreSQL database along with the PgAdmin tool using Docker Compose. With just a few commands, you can have a fully functional PostgreSQL environment running in seconds.

## Prerequisites

Make sure you have the following installed on your machine:

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Quickstart

Follow these steps to quickly launch PostgreSQL and PgAdmin:

1. Clone this repository:

    ```bash
    git clone https://github.com/EriksonMurrugarra/postgres-pgadmin-docker-setup.git
    cd postgres-pgadmin-docker-setup
    ```

2. Launch the containers:

    ```bash
    docker-compose up -d
    ```

    This will start both PostgreSQL and PgAdmin in detached mode.

3. Access PgAdmin:

   Once the containers are running, you can access PgAdmin via your web browser by visiting:

   ```
   http://localhost:8888
   ```

4. Login to PgAdmin:

    Use the following credentials to login:

    - **Email**: `pgadmin@example.com`
    - **Password**: `admin`

5. Add a new PostgreSQL server in PgAdmin:

    - **Host**: `postgres`
    - **Port**: `5432`
    - **Username**: `admin`
    - **Password**: `admin`

You are now ready to manage your PostgreSQL database!

## Docker Compose Configuration

Here’s the content of the `docker-compose.yml` file used in this setup:

```yaml
version: '3.9'

services:
  postgres:
    image: postgres:14-alpine
    ports:
      - 5432:5432
    volumes:
      - myDatabaseVol:/var/lib/postgresql/data
    environment:
      - POSTGRES_PASSWORD=admin
      - POSTGRES_USER=admin
      - POSTGRES_DB=mydb

  pgadmin:
    image: dpage/pgadmin4
    restart: always
    ports:
      - "8888:80"
    environment:
      PGADMIN_DEFAULT_EMAIL: pgadmin@example.com
      PGADMIN_DEFAULT_PASSWORD: admin
    volumes:
      - pgAdminVol:/var/lib/pgadmin
    depends_on:
      - postgres

volumes:
  myDatabaseVol:
  pgAdminVol:
```

### Explanation:

- **Postgres Service**: Runs the PostgreSQL database, accessible via port `5432` on the host machine. The database is named `mydb`, and the default user is `admin` with the password `admin`.
  
- **PgAdmin Service**: Runs PgAdmin, accessible via port `8888` on your browser. The default login credentials for PgAdmin are provided as environment variables.

## Stopping the Containers

To stop and remove the containers, run:

```bash
docker-compose down
```

This will stop the containers and remove the network. The data stored in the database will persist across restarts due to the volume configuration.

## Additional Commands

- To view container logs:

  ```bash
  docker-compose logs -f
  ```

- To stop the containers without removing them:

  ```bash
  docker-compose stop
  ```

- To remove containers and volumes:

  ```bash
  docker-compose down -v
  ```

## Troubleshooting

- If you encounter any issues with container startup, check the logs by running:

  ```bash
  docker-compose logs
  ```

- Make sure the required ports (`5432` for Postgres and `8888` for PgAdmin) are available and not being used by other applications.