# Upstream's Dockerfile (app/Dockerfile) with these changes:
# - the database settings and the start command from upstream's docker-compose.yml are baked in
#   (ENV POSTGRES_*, database at host `db`; CMD runs the migrations, then uvicorn on 8091,
#   without --reload since the source is in the image rather than bind-mounted);
# - the paths read app/ (COPY app/pyproject.toml ..., COPY app/app ./app).
# Upstream's compose file also runs this container privileged with SYS_ADMIN; the lab does
# not (the intended path to root, sudo find, works without it).
FROM python:3.10-bookworm as builder

RUN pip install poetry==1.4.2
WORKDIR /app

COPY app/pyproject.toml app/poetry.lock ./
RUN poetry export -f requirements.txt --output requirements.txt --without-hashes


FROM python:3.10-slim-bookworm as runtime

RUN apt-get update
RUN apt-get -y install libpq-dev gcc vim sudo

COPY --from=builder /app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app/app ./app
WORKDIR /app

# our Chef sometimes needs to find some files on the filesystem
# we're allowing to run find with root permissions via sudo
# in this way, our Chef is able to search everywhere across the filesystem
RUN echo 'ALL ALL=(ALL) NOPASSWD: /usr/bin/find' | sudo tee /etc/sudoers.d/find_nopasswd > /dev/null

# for security, we're creating a dedicated non-root user
RUN useradd -m app
RUN chown app .
USER app

ENV POSTGRES_USER=admin \
    POSTGRES_PASSWORD=password \
    POSTGRES_SERVER=db \
    POSTGRES_PORT=5432 \
    POSTGRES_DB=restaurant
EXPOSE 8091
CMD ["bash", "-c", "alembic upgrade head && uvicorn main:app --host 0.0.0.0 --port 8091 --workers 1"]
