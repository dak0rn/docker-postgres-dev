# docker-postgres-dev
Development setup for Postgres in Docker

Self-configuring Postgres container that can be used for development setups.
Automatically creates the database user with a password, and the database
and installs the `uuid-ossp` extension upon startup.

## Usage

Copy the `postgres/` folder into your project. The easiest integration can be achieved with
docker-compose:

```yml
version: '3'
services:
    postgres:
        build:
            context: ./postgres
            args:
                DBUSER: scott
                DBPASS: tiger
                DBNAME: productiondatabase
        networks:
            - applicationnetwork
        ports:
            - "5432:5432"
        volumes:
            - ./postgres/data:/var/lib/postgresql
        hostname: postgres

networks:
    applicationnetwork:
```

The build parameters are used to bootstrap the database. It's contents are stored
in the `postgres/data` folder - remove that very folder to "reset" the database.

## Caveat

**Does not work on Windows** if you keep the Postgres files on a volume. That is,
remove the `volume` entry to make it work on Windows, too. However, since the files
are then stored inside of the container, a `docker-compose down` would remove all your
data, too (use `docker-compose stop` instead, btw).
