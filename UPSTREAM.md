# Upstream

| | |
| --- | --- |
| Project | Damn Vulnerable RESTaurant API Game |
| Repository | https://github.com/theowni/Damn-Vulnerable-RESTaurant-API-Game |
| Version | main (no release tags) |
| Commit | d32d09972aec7830f5f1d4a5d274cac7aae7eb82 |
| Licence | GPL-3.0 |

`build/web/app/` is that commit, unchanged, without its Git history. `build/web/Dockerfile` is
upstream's Dockerfile with the database settings and start command from upstream's
`docker-compose.yml` baked in (migrations, then uvicorn on 8091 without `--reload`).
Dependencies come from upstream's `poetry.lock`. Upstream's compose file runs the API container
privileged with SYS_ADMIN; the lab does not. `build/db/Dockerfile` is upstream's
`postgres:15.4-alpine` with its compose environment baked in. To update, replace
`build/web/app/` with a newer commit, then change this table.
