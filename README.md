# replicaSet

Docker-based MongoDB replica set for local development. It provisions three MongoDB nodes plus an arbiter and runs an initialization script to create the replica set and a sample user.

## Prerequisites

- Docker and Docker Compose
- `openssl` (for generating the keyfile)

## Quick start

1. Create a keyfile (required for internal authentication):

   ```bash
   openssl rand -base64 756 > keyfile
   chmod 400 keyfile
   ```

2. Start the stack:

   ```bash
   docker compose up -d
   ```

3. Watch the setup container:

   ```bash
   docker compose logs -f mongo-setup
   ```

4. Connect to the primary:

   ```bash
   mongosh -u root -p secret --authenticationDatabase admin --host localhost:27018
   ```

## Ports

- `mongo1` → `localhost:27018`
- `mongo2` → `localhost:27019`
- `mongo3` → `localhost:27020`

## Configuration notes

- The replica set name is `rs0` in `docker-compose.yml` and `setup_mongo.sh`.
- Default credentials are `root / secret` (admin database).
- The initialization script creates an additional user `otheradmin` with password `othersecret`.
- If you change container names, ports, or credentials, update `setup_mongo.sh` accordingly.

## Useful commands

Check replica set status:

```bash
docker compose exec mongo1 mongosh -u root -p secret --authenticationDatabase admin --eval 'rs.status()'
```
