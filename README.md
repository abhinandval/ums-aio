# UMS-AIO (Universal Media Server - All In One)

A portable Docker-based media server stack including Jellyfin, Sonarr, Radarr, qBittorrent, Prowlarr, Whisparr, and FlareSolverr.

## Platform Setup

Select your operating system to get started:

- **[macOS](README.mac.md)** - Homebrew + Colima setup guide
- **Linux** - *Coming soon*
- **Windows** - *Coming soon*

---

## Overview

### Services Included

| Service | Port | Description |
|---------|------|-------------|
| **Jellyfin** | 8096 | Media server for streaming movies and TV shows |
| **Sonarr** | 8989 | TV show automation and management |
| **Radarr** | 7878 | Movie automation and management |
| **Whisparr** | 6969 | Adult content automation and management |
| **qBittorrent** | 8080 | Torrent download client |
| **Prowlarr** | 9696 | Indexer manager for Sonarr and Radarr |
| **FlareSolverr** | 8191 | Cloudflare proxy solver for indexers |

### Portability

This setup is designed to be **completely portable**:

- All configurations stored in `./data/`
- All media stored in `./media/`
- Uses relative paths - works on any machine
- Simply copy the entire folder to move to another system

### Project Structure

```
ums-aio/
├── docker-compose.yml      # Service orchestration
├── README.md               # This file
├── README.mac.md           # macOS setup guide
├── AGENTS.md               # Contribution guidelines
├── data/                   # Service configurations (gitignored)
│   ├── jellyfin/
│   ├── sonarr/
│   ├── radarr/
│   ├── whisparr/
│   ├── qbittorrent/
│   ├── prowlarr/
│   └── flaresolverr/
└── media/                  # Media storage (gitignored)
    ├── movies/
    ├── tv_shows/
    └── downloads/
```

## Common Commands

```bash
# Start all services
docker compose up -d

# Stop all services
docker compose down

# View logs
docker compose logs -f

# Update all containers
docker compose pull && docker compose up -d

# Restart a specific service
docker compose restart jellyfin
```

## Configuration

All services mount `./media` as `/storage`. Configure paths in each service's web UI:

- **Jellyfin**: Add `/storage/movies` and `/storage/tv_shows` as libraries
- **Sonarr**: Root folder `/storage/tv_shows`, downloads `/storage/downloads`
- **Radarr**: Root folder `/storage/movies`, downloads `/storage/downloads`
- **Whisparr**: Root folder `/storage/adult`
- **qBittorrent**: Default save path `/storage/downloads`
- **Prowlarr**: Configure FlareSolverr at `http://flaresolverr:8191`

## Contributing

This project follows [Conventional Commits](AGENTS.md). Please read the guidelines before contributing.

## License

MIT License - feel free to use and modify as needed.
