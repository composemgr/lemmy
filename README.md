# Lemmy

A self-hosted lemmy application.

## Installation

### Option 1: Quick Install
```bash
curl -q -LSsf "https://raw.githubusercontent.com/composemgr/lemmy/main/docker-compose.yaml" | docker compose -f - up -d
```

### Option 2: Git Clone
```bash
git clone "https://github.com/composemgr/lemmy" ~/.local/srv/docker/lemmy
cd ~/.local/srv/docker/lemmy
docker compose up -d
```

### Option 3: Using composemgr
```bash
composemgr install lemmy
```

## Configuration

See docker-compose.yaml for environment variables and configuration options.

## Documentation

Check the official project documentation for detailed setup and usage information.
