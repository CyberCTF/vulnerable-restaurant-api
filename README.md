# Damn Vulnerable RESTaurant API Game

[Damn Vulnerable RESTaurant](https://github.com/theowni/Damn-Vulnerable-RESTaurant-API-Game) by
Krzysztof Pranczk (theowni): an intentionally vulnerable restaurant API built with FastAPI, where
you start as a low-privileged API user and escalate to root on the server. This repository runs
it with [Isoloom](https://www.isoloom.com): [`isoloom.yml`](isoloom.yml) describes the machines,
and the upstream source in [`build/web/app/`](build/web/app) builds with its own Dockerfile
(database settings and start command baked in).

| Machine | Service |
| --- | --- |
| web | RESTaurant API on port 8091, Swagger at `/docs`, Redoc at `/redoc` |
| db | PostgreSQL 15 on port 5432 |

Upstream's compose file runs the API container privileged; this lab does not, since the
intended path to root does not need it. Upstream's developer game mode (`game.py`, fixing the
vulnerabilities in a bind-mounted source tree) is not part of this lab.

## Run it

```bash
isoloom generate
isoloom run docker
```

Then open http://localhost:8091/docs. The same spec runs as Docker on a local VM (`docker-vm`),
on a cloud VM (`cloud-docker`) or on Kubernetes. Lab guide:
[upstream's README](https://github.com/theowni/Damn-Vulnerable-RESTaurant-API-Game#readme).

Upstream version and commit: [UPSTREAM.md](UPSTREAM.md).

## Licence

GPL-3.0, as Damn Vulnerable RESTaurant ([LICENSE](LICENSE)). This application is deliberately
vulnerable: keep it isolated.
