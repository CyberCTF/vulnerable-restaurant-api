# The database upstream's docker-compose.yml runs: postgres:15.4-alpine with its environment
# baked in.
FROM postgres:15.4-alpine
ENV POSTGRES_USER=admin \
    POSTGRES_PASSWORD=password \
    POSTGRES_DB=restaurant \
    PGDATA=/var/lib/postgresql/data/pgdata
