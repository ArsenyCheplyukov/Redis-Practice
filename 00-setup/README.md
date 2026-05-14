# 00 — Setup

Redis playground via Docker Compose.

## Why Compose, not `docker run`

- Version pinned in a file under version control
- Volumes and config visible without re-typing flags
- `up -d` / `down` instead of remembering port mappings
- Future env vars / config flags land in one place

## Run

```bash
cd 00-setup
docker compose up -d
```

Verify:

```bash
docker exec -it redis-playground redis-cli ping
```

Expected: `PONG`

Enter interactive shell:

```bash
docker exec -it redis-playground redis-cli
```

## Stop

```bash
docker compose down       # keeps data volume
docker compose down -v    # wipes data volume too
```

## Image notes

`redis:8-alpine` — major version 8, Alpine base (~30 MB vs ~120 MB for the
full image). Pinning to `8-alpine` rather than `latest` so behavior doesn't
drift if Redis 9 ships later.

## Data persistence

Volume `redis-data` is mounted at `/data` inside the container. Redis writes
RDB snapshots there by default — data survives container restart but is wiped
by `docker compose down -v`.

## Port

Host `6379` → container `6379`. If something else is bound to `6379` locally,
edit the left-hand side of the mapping in `docker-compose.yml`.
