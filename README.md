## 👋 Welcome to lemmy 🚀

A self-hosted, federated social news aggregator

## 📋 Description

Lemmy is a federated link aggregator and social network. The UI container
serves the web frontend and proxies API calls to the Lemmy server, which holds
the instance data in Postgres. Instances can federate with each other over
ActivityPub.

## 🚀 Services

- **app**: dessalines/lemmy-ui:nightly
- **server**: dessalines/lemmy:nightly

### Infrastructure Components

- **db**: Postgres database

## 📦 Installation

### Option 1: Quick Install

```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/lemmy/main/docker-compose.yaml" -o compose.yml
```

### Option 2: Git Clone

```bash
git clone "https://github.com/composemgr/lemmy" ~/.local/srv/docker/lemmy
cd ~/.local/srv/docker/lemmy
cp default.env .env
docker compose --env-file .env up -d
```

### Option 3: Using composemgr

```bash
composemgr install lemmy
```

## 🔧 Configuration

### Environment Variables

```shell
TZ=America/New_York
DB_USER_NAME=lemmy
DB_USER_PASS=replace-with-a-random-password
DB_CREATE_DATABASE_NAME=lemmy
```

See `docker-compose.yaml` for complete list of configurable options.

### Environment Files

`default.env.sample` holds the full set of variables and is copied to `.env`
before starting the stack:

```bash
cp default.env .env
docker compose --env-file .env up -d
```

`app.env.sample` holds the app-specific overrides and is copied to `app.env`:

```bash
cp app.env.sample app.env
```

`composemgr up` applies `app.env` and `default.env` automatically. Raw
`docker compose` commands need `--env-file .env` to pick up the same values.

### Reverse Proxy

Three standalone compose files ship in this repo:

| File | Use |
|------|-----|
| `docker-compose.yaml` | Default, published on `172.17.0.1:64231` |
| `docker-compose.traefik.yaml` | Behind an existing external `traefik` network |
| `docker-compose.tunnel.yaml` | Behind an existing external `cloudflare` tunnel network |

```bash
docker compose -f docker-compose.traefik.yaml --env-file .env up -d
```

The proxy variants publish no host ports. The UI is reachable only through the
proxy network. The Lemmy server and Postgres stay internal to the project
network in every variant.

## 🌐 Access

- **Web Interface**: http://172.17.0.1:64231

Create the instance administrator in the browser on first load. The admin
username is the value of `LEMMY_UI_LEMMY_EXTERNAL_HOST`, which is built from
`BASE_HOST_NAME`, so set that to the public hostname before the first start.

## 📂 Volumes

- `./volumes/config/lemmy` - Rendered Lemmy server configuration
- `./volumes/db/postgres/lemmy` - Postgres database data

## 🔐 Security

- Change all default passwords before deploying to production
- Use strong secrets for all authentication tokens
- Configure HTTPS using a reverse proxy (nginx, traefik, caddy)
- Regularly update Docker images for security patches
- Backup your data regularly

Lemmy is a public-facing social application. It accepts signups and federated
traffic from other instances by default. Review the registration and federation
settings right after the first start, and set `BASE_HOST_NAME` before it so
ActivityPub callbacks and password reset links resolve to the right host.

## 🔍 Logging

```shell
docker compose --env-file .env logs -f app
docker compose --env-file .env logs -f server
```

## 🛠️ Management

```bash
# Start services
docker compose --env-file .env up -d

# Stop services
docker compose --env-file .env down

# Update to latest images
docker compose --env-file .env pull && docker compose --env-file .env up -d

# View logs
docker compose --env-file .env logs -f

# Restart services
docker compose --env-file .env restart
```

## 📋 Requirements

- Docker Engine 20.10+
- Docker Compose V2+

## 🤝 Author

🤖 casjay: [Github](https://github.com/casjay) 🤖  
🦄 composemgr: [Github](https://github.com/composemgr) 🦄
