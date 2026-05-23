# beammp-server-docker

Docker image for [BeamMP-Server](https://github.com/BeamMP/BeamMP-Server) based on `debian:bookworm-slim`.

## Tags

For a list of tags, see [jwgesshoo/beammp-server](https://hub.docker.com/r/jwgesshoo/beammp-server/tags) on Docker Hub.
Images are also published to the [GitHub Container Registry](https://ghcr.io/JoshuaGessner/beammp-server-docker).

## Usage

### Running a basic container with persistent data

```sh
docker run -itd \
  --init \
  -p 30814:30814/tcp \
  -p 30814:30814/udp \
  --name beammp-server \
  -v /data/beammp-server:/data \
  jwgesshoo/beammp-server
```

### Server configuration with Environment Variables

Copy `beammp-server.env`, fill in your `BEAMMP_AUTH_KEY`, and pass it to the container:

```sh
docker run -itd \
  --init \
  -p 30814:30814/tcp \
  -p 30814:30814/udp \
  --name beammp-server \
  --env-file beammp-server.env \
  -v /data/beammp-server:/data \
  jwgesshoo/beammp-server
```

You can also pass individual environment variables to override specific settings:

```sh
docker run -itd \
  --init \
  -p 30814:30814/tcp \
  -p 30814:30814/udp \
  --name beammp-server \
  --env-file beammp-server.env \
  -e BEAMMP_MAP="/levels/west_coast_usa/info.json" \
  -v /data/beammp-server:/data \
  jwgesshoo/beammp-server
```

### Using Docker Compose

An example `docker-compose.yml` is included. Copy and edit `beammp-server.env` then run:

```sh
docker compose up -d
```

## Environment Variables

See `beammp-server.env` for the full list of supported variables.
Full documentation: https://docs.beammp.com/server/manual/

| Variable | Default | Description |
|---|---|---|
| `BEAMMP_AUTH_KEY` | *(empty)* | **Required.** Your BeamMP authentication key from https://beammp.com/k/dashboard |
| `BEAMMP_NAME` | `BeamMP Server` | Server name shown in the server list |
| `BEAMMP_MAP` | `/levels/gridmap_v2/info.json` | Map to load |
| `BEAMMP_MAX_PLAYERS` | `8` | Maximum number of players |
| `BEAMMP_MAX_CARS` | `1` | Maximum cars per player |
| `BEAMMP_PORT` | `30814` | Port the server listens on |
| `BEAMMP_PRIVATE` | `true` | Hide the server from the public list |
| `BEAMMP_DEBUG` | `false` | Enable debug logging |
| `BEAMMP_LOG_CHAT` | `true` | Log chat messages |
| `BEAMMP_DESCRIPTION` | `BeamMP Default Description` | Server description |
| `BEAMMP_TAGS` | `Freeroam` | Comma-separated list of tags |
| `BEAMMP_RESOURCE_FOLDER` | `Resources` | Folder for server-side mods |

## Building

To build the image yourself:

```sh
docker build \
  --build-arg BMPS_VER="v3.9.3" \
  -t beammp-server:3.9.3 .
```

Leave `BMPS_VER` unset to automatically use the latest release:

```sh
docker build -t beammp-server:latest .
```

### Multi-arch build

```sh
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  --build-arg BMPS_VER="v3.9.3" \
  -t beammp-server:3.9.3 .
```
