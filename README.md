# Exercise Report
- https://github.com/ak5154639/fs-containers

# Blog App

## Requirements

- Docker Engine with the Docker Compose plugin.
- In WSL, Docker Desktop must be running with WSL integration enabled, or a Docker Engine must be installed and running inside WSL.

Run the commands below from the repository root in your WSL terminal.

## Development

Build and start the development frontend, backend, and local MongoDB:

```bash
docker compose -f docker-compose.dev.yml up --build -d
```

Open <http://localhost:8081>. Development MongoDB is available on `localhost:27017` and its data is stored in a Docker volume.

Follow development logs:

```bash
docker compose -f docker-compose.dev.yml logs -f
```

Stop the development stack while keeping its database:

```bash
docker compose -f docker-compose.dev.yml down
```

## Production

Create a local production JWT secret once. The `.env` file is ignored by Git; keep it private. The command preserves an existing `.env`:

```bash
if [ ! -f .env ]; then
	printf 'BLOGAPP_SECRET=%s\n' "$(openssl rand -hex 32)" > .env
fi
```

Build and start production:

```bash
docker compose -f docker-compose.yml up --build -d
```

Open <http://localhost:8080>. Production MongoDB is available on `localhost:27018`, separate from the development database.

Follow production logs:

```bash
docker compose -f docker-compose.yml logs -f
```

Stop production while keeping its database:

```bash
docker compose -f docker-compose.yml down
```

Both stacks use persistent Docker volumes. Avoid `docker compose down -v` unless you intend to delete that stack's database data.
